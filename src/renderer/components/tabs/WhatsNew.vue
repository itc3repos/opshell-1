<template>
  <div id="whatsnew">
    <div class="versionwrapper" v-for="version in changes" v-bind:key="version.version">
      <span class="header" v-on:click="showVersion(version.version)">
        <i class="fa" v-bind:class="{'fa-caret-right':!versions[version.version],'fa-caret-down':versions[version.version]}"></i>
        Version {{version.version}} - {{version.releaseDate}}
      </span>

      <div v-if="versions[version.version] && version.added.length">
        <div class="subheader"><i class="fa fa-plus-square-o"></i> Added:</div>
        <div class="subitem" v-for="add in version.added" v-bind:key="add">
          - {{add}}<br />
        </div>
      </div>

      <div v-if="versions[version.version] && version.fixed.length">
        <div class="subheader"><i class="fa fa-bug"></i> Fixed:</div>
        <div class="subitem" v-for="fix in version.fixed" v-bind:key="fix">
          - {{fix}}<br />
        </div>
      </div>

      <div v-if="versions[version.version] && version.changed.length">
        <div class="subheader"><i class="fa fa-exchange"></i> Changed:</div>
        <div class="subitem" v-for="change in version.changed" v-bind:key="change">
          - {{change}}<br />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'whatsNew',
  data: function() {
    return {
      versions: {},
      currentVersion: '0.0.8',
      changes:
      [
        {
          'version': '0.0.8',
          'releaseDate': '2017-10-09',
          'added': [
            'Ability to set the default ssh user to a custom user instead of the standard AWS users',
            'Added a "What\'s New" menu item, to show changelog.  Will open first time app is opened when updated'
          ],
          'fixed': [
            'SSH through a bastion host was not working properly due to a change in 0.0.7',
            'Tab headers now stay in place at top of screen when scrolling tab content'
          ],
          'changed': [
            'Removed all modals except for the about window.   All configuration has been moved into a tab',
            'Removed most dom manipulation via jQuery in favor of Vue methods'
          ]
        },
        {
          'version': '0.0.7',
          'releaseDate': '2017-10-07',
          'added': [
            'Ability to add users to a region to ssh as instead of just the default aws users',
            'If no ssh key is uploaded it will try to do password authentication'
          ],
          'fixed': [
          ],
          'changed': [
          ]
        },
        {
          'version': '0.0.6',
          'releaseDate': '2017-10-06',
          'added': [
            'About page',
            'Check for newer versions'
          ],
          'fixed': [
            'Bastion Host was not properly saving'
          ],
          'changed': [
          ]
        },
        {
          'version': '0.0.5',
          'releaseDate': '2017-10-05',
          'added': [
            'Added the ability to delete an Organization'
          ],
          'fixed': [
            'Deleting a region was not deleting imported keys'
          ],
          'changed': [
          ]
        },
        {
          'version': '0.0.4',
          'releaseDate': '2017-10-02',
          'added': [
          ],
          'fixed': [
          ],
          'changed': [
            'Renamed to Opshell, which changes file and db storage locations.  No migration for this, since it has only been publically available for a couple of days.'
          ]
        },
        {
          'version': '0.0.3',
          'releaseDate': '2017-09-30',
          'added': [
          ],
          'fixed': [
            'General UI fixes with selected options not showing selected by default',
            'Fixed an issue with running processes such as vi or top within the terminal'
          ],
          'changed': [
          ]
        },
        {
          'version': '0.0.2',
          'releaseDate': '2017-09-30',
          'added': [
            'Ability to filter listed Instances by a search string',
            'Sort Instances by clicking on column header'
          ],
          'fixed': [
          ],
          'changed': [
          ]
        },
        {
          'version': '0.0.1',
          'releaseDate': '2017-09-28',
          'added': [
            'Add Organizations',
            'Add Regions to Organizations',
            'Add AWS Access Keys to Regions',
            'Scan for Required SSH Keys in a Region',
            'Import/Update SSH Keys',
            'List Instances in a Region',
            'SSH to an Instance',
            'Add a Bastion Host to a Region',
            'SSH to an Instance through a Bastion Host',
            'Remember what user / ip was used to connect to an Instance'
          ],
          'fixed': [
          ],
          'changed': [
          ]
        }
      ]
    };
  },
  mounted: async function () {
    await this.$db.tempData.update({type: 'whatsnew'}, {type: 'whatsnew', version: this.currentVersion}, {upsert: true});
  },
  methods: {
    showVersion: function(version) {
      if (this.versions[version]) {
        this.$set(this.versions, version, false);
      } else {
        this.$set(this.versions, version, true);
      }
    }
  }
};
</script>
<style>
@import '../../assets/css/whatsnew.css'
</style>