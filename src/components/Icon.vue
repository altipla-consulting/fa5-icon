
<template></template>

<script>
import { h, mergeProps } from 'vue'
import { icon as faIcon } from '@fortawesome/fontawesome-svg-core'


function convertTag(input, attrs) {
  return h(input.tag, mergeProps(input.attributes, attrs), input.children ? input.children.map(child => convertTag(child)) : [])
}


export default {
  name: 'ac-icon',

  props: {
    variant: String,
    regular: Boolean,
    solid: Boolean,
    light: Boolean,
    brands: Boolean,
    duotone: Boolean,

    fixedWidth: Boolean,
    spin: Boolean,
    pulse: Boolean,

    size: String,
    transform: String,
  },

  setup(props, { slots, attrs }) {
    return () => {
      let variants = {
        regular: 'far',
        solid: 'fas',
        light: 'fal',
        brands: 'fab',
        duotone: 'fad',
      }
      let prefix = props.variant || Object.keys(variants).find(key => props[key])
      prefix = variants[prefix]

      let classes = {
        spin: 'fa-spin',
        pulse: 'fa-pulse',
        fixedWidth: 'fa-fw',
        size: `fa-${props.size}`,
      }
      classes = Object.keys(classes)
        .map(key => props[key] ? classes[key] : null)
        .filter(key => key)

      let iconName = slots.default()[0].children.trim()
      let icon = faIcon({ prefix, iconName }, { classes })
      if (!icon) {
        throw new Error(`could not find icon: ${prefix}-${iconName}`)
      }

      return convertTag(icon.abstract[0], attrs)
    }
  },
}
</script>
