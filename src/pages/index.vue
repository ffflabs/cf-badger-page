
<template>
  <Hero />

  <section class="block text-gray-700">
    <div
      class="relative flex-grow my-0 mx-auto w-auto"
      style="max-width: 1344px;"
    >
      <Intro />
      <div class="justify-center -m-3" style="justify-content: center">
        <div
          class="px-1 pt-4 pb-4 mx-auto mb-0 -mt-8 max-w-3xl "
          style="flex-basis: 0px;"
        >
          <repo-and-buttons>
            <input
              id="repoInput"
              ref="repoInput"
              v-model="repository"
              class="inline-flex relative px-2 justify-start items-center m-0 w-full max-w-full h-8 font-sans text-sm leading-normal text-gray-800 align-top rounded border border-gray-400 border-solid appearance-none cursor-text shadow-xs focus:border-blue-600 focus:shadow-xs hover:border-gray-500"
              type="text"
              placeholder="ffflabs/cf-badger"
              data-placeholder="ffflabs/cf-badger"
              data-valid-example="ffflabs/cf-badger"
              @change="getWorkFlows"
            />
          </repo-and-buttons>
          <form-line>
            <div class="col-span-6 sm:col-span-3">
              <label class="block text-md  text-left text-gray-800 cursor-default" :style="WorkflowTitle.color">
                {{ WorkflowTitle.content }}
              </label>
              <div
                class="clear-both relative text-base text-left box-border"
              >
                <select
                  v-model="workflow"
                  :disabled="!workflows.length"
                  class="block relative justify-start items-center pr-10 m-0 w-full max-w-full h-8 font-sans leading-normal text-gray-800 align-top whitespace-pre rounded border border-gray-400 border-solid shadow-none appearance-none cursor-pointer focus:border-blue-600 focus:shadow-xs hover:border-gray-500"
                  style="outline: 0px;"
                  @change="pickWorkFlow"
                >
                  <option
                    v-if="!workflows.length"
                    class="py-4 px-px w-full leading-5 whitespace-no-wrap"
                  >
                    No workflows discovered yet
                  </option>
                  <option
                    v-for="workflow in workflows"
                    :key="workflow.id"
                    :title="workflow.name"
                    :value="workflow"
                    class="py-4 px-px w-full leading-5 whitespace-no-wrap"
                  >
                    {{ workflow.name }}
                  </option>
                </select>
              </div>
            </div>
            <div class="col-span-6 sm:col-span-3">
              <label
                class="block text-md  text-left text-gray-800 cursor-default"
                :style="BranchesTitle.color"
              >{{ BranchesTitle.content }}</label>
              <div class="mb-0">
                <div
                  class="clear-both relative text-base text-left box-border"
                >
                  <select
                    v-model="branch"
                    :disabled="!branches.length"
                    class="block relative justify-start items-center pr-10 m-0 w-full max-w-full h-8 font-sans leading-normal text-gray-800 align-top whitespace-pre rounded border border-gray-400 border-solid shadow-none appearance-none cursor-pointer focus:border-blue-600 focus:shadow-xs hover:border-gray-500"
                    style="outline: 0px;"
                  >
                    <option
                      v-if="!branches.length"
                      class="py-4 px-px w-full leading-5 whitespace-no-wrap"
                    >
                      No branches discovered yet
                    </option>
                    <option
                      v-for="branch in branches"
                      :key="branch.id"
                      :value="branch.head_branch"
                      class="py-4 px-px w-full leading-5 whitespace-no-wrap"
                    >
                      {{ branch.head_branch }}
                    </option>
                  </select>
                </div>
              </div>
            </div>
          </form-line>
          <BadgeAndPreview :href="gotoURL" :src="badgeURL">
            <badge-style :parent_style="style" @change="emitChange" />
          </BadgeAndPreview>
          <MarkdownBox :markdown-source="markdownSource" />
        </div>
      </div>
    </div>
  </section>
</template>

<route lang="yaml">
name: home
meta:
  layout: home
</route>
<script lang="ts">
import { defineComponent, ref } from 'vue'
import MarkdownBox from '~/components/MarkdownBox.vue'

const people = [
  { id: 1, name: 'Wade Cooper' },
  { id: 2, name: 'Arlene Mccoy' },
  { id: 3, name: 'Devon Webb' },
  { id: 4, name: 'Tom Cook' },
  { id: 5, name: 'Tanya Fox' },
  { id: 6, name: 'Hellen Schmidt' },
  { id: 7, name: 'Caroline Schultz' },
  { id: 8, name: 'Mason Heaney' },
  { id: 9, name: 'Claudie Smitham' },
  { id: 10, name: 'Emil Schaefer' },
]

const BadgerComponent = defineComponent({

  setup() {
    const selected = ref(people[3])

    return {
      people,
      selected,
    }
  },
  data() {
    return {
      showHtml: false,
      repository: '',
      modalActive: false,
      style: { name: 'for-the-badge', id: 'for-the-badge' },
      mask: 'W/R',
      branch: '',
      privateRepo: false,
      token: '',
      workflows: [],
      owner: '',
      workflow: {},
      hashHex: '',
      branches: [],
      tokenInputType: 'password',
      login: '',
      installations: [],
      repos: [],
      maskedInput: null
    }
  },
  computed: {
    WorkflowTitle() {
      return this.workflows.length
        ? { content: 'WorkFlow', color: 'color:black' }
        : {
          content: 'Enter token to enable selection',
          color: 'color:#aaa',
        }
    },
    BranchesTitle() {
      return this.branches.length
        ? { content: 'Branch', color: 'color:black' }
        : {
          content: 'Pick a Workflow to enable selection',
          color: 'color:#aaa',
        }
    },
    badgeURL() {
      const style = this.style || ''

      const badgeBaseUrl = this.hashHex
        ? this.endpointUrl
        : `${location.protocol}//${location.host}/images`
      return `${badgeBaseUrl}/endpoint.svg?branch=${this.branch || 'master'
      }&style=${style}`
    },
    endpointUrl() {
      return this.hashHex
        ? `${location.protocol}//${location.host}/badger/${this.hashHex}`
        : `${this.repoUrl}/${this.workflow.id || 0}`
    },
    viewerUrl() {
      return `${location.protocol}//${location.host}/badger`
    },
    repoUrl() {
      this.repository
            = this.repository
            || location.hash.replace('#', '')
            || 'ffflabs/cf-badger'
      return `${location.protocol}//${location.host}/badger/${this.repository}`
    },
    gotoURL() {
      return this.workflow.filename_url || ''
    },
    markdownSource() {
      return `[![${this.workflow.name || 'N/A'}](${this.badgeURL})](${this.gotoURL
      })`
    },
    htmlSource() {
      return (
        `<a href="${
          this.gotoURL
        }">`
            + `<img alt="Build Status" src="${
              this.badgeURL
            }" />`
            + '</a>'
      )
    },

  },
  mounted() {
    globalThis.BadgerComponent = this
  },
  methods: {
    toggleModal() {
      this.modalActive = !this.modalActive
    },
    emitChange(newStyle) {
      this.style = ref(newStyle)
      console.log({ emitChange: newStyle, style: this.style })
    }
  }

})
export default BadgerComponent

/*

const WorkflowTitle = computed(
  () {
    return workflows.value.length
      ? { content: 'WorkFlow', color: 'color:black' }
      : {
        content: 'Enter token to enable selection',
        color: 'color:#aaa',
      }
  }
)
*,
*/
</script>
