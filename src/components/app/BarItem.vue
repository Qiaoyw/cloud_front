<script>
  import { VHover, VListItem } from 'vuetify/lib'

  export default {
    name: 'AppBarItem',

    render (h) {
      return h(VHover, {
        scopedSlots: {
          default: ({ hover }) => {
            return h(VListItem, {
              attrs: this.$attrs,
              class: {
                'black--text': !hover,
                'white--text primary elevation-18': hover,
                'profilefont':!hover,
              },
              props: {
                activeClass: '',
                dark: hover,
                link: true,
                ...this.$attrs,
              },
            }, this.$slots.default)
          },
        },
      })
    },
  }
</script>
<style>
  .profilefont{
    font-size: 32px;
  }
</style>