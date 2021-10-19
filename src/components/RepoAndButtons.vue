<template>
  <form-line>
    <div class="col-span-6 sm:col-span-3">
      <label
        class="block text-md  text-left text-gray-800 cursor-default"
      >Repository</label>
      <div class="mb-0">
        <div
          class="clear-both relative text-base text-left box-border"
        >
          <slot></slot>
        </div>
      </div>
    </div>

    <div class="col-span-6 sm:col-span-3">
      <label
        class="block text-md  text-left text-gray-800 cursor-default"
      >Connect to Github</label>
      <div class="mb-0">
        <div
          class="clear-both relative text-base text-left box-border"
        >
          <a
            class="inline-flex items-center px-3  border border-transparent shadow-sm text-sm leading-4 font-medium rounded-md text-white bg-gray-600 hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
            style="width: calc(50% - 3.5em); background-image: linear-gradient(rgb(67, 76, 84) 0%, rgb(57, 65, 72) 35%, rgb(37, 42, 48) 100%);"
            href="/bdg/install"
          >
            <carbon-logo-github style="height:1.75rem;width:1.75rem" class="pr-2 text-base h-7 w-7" />

            Install App
          </a>
          <span style="position:relative;top:-8px">
            &nbsp; or &nbsp;
          </span>

          <a
            class="inline-flex items-center px-3  border  shadow-sm text-sm leading-4 font-medium rounded-md text-gray-800 no-underline align-top rounded border border-gray-400 border-solid shadow-none appearance-none cursor-pointer select-none whitespace-no-wrap hover:border-gray-500 hover:text-gray-800 focus:border-blue-600 focus:text-gray-800"
            style="margin-top:-0.05em;                              width:calc(50% )                            "
            href="/bdg/oauth"
          >
            <carbon-logo-github style="height:1.75rem;width:1.75rem" class="pr-2 h-7 w-7" />

            Login with Github
          </a>
        </div>
      </div>
    </div>
  </form-line>
</template>
