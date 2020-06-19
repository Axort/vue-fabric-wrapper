<script>
import fabricObject from "./fabricObject";
import fabricCollection from "./fabricCollection";
export default {
  name: "fabric-group",
  provide() {
    return {
      $group: () => this.groupDef
    };
  },
  mixins: [fabricObject, fabricCollection],
  props: {
    subTargetCheck: {
      type: Boolean,
      default: false
    },
    addWithoutUpdate: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      groupDef: null,
      type: "group"
    };
  },
  render(h) {
    return this.$slots.default ? h("div", this.$slots.default) : undefined;
  },
  watch: {
    parentItem: {
      handler(newValue) {
        if (newValue) {
          //Parent is created
          this.groupDef = new this.fabric.Group([], { ...this.definedProps });
          if (this.parentType == "group") {
            // eslint-disable-next-line no-console
            console.error("AddWithoutUpdate1", this, this.parentItem);
            if (this.parentItem.addWithoutUpdate) {
              this.parentItem.add(this.groupDef);
              // eslint-disable-next-line no-console
              console.error("AddWithoutUpdate");
            } else {
              this.parentItem.addWithUpdate(this.groupDef);
            }
          } else {
            this.canvas.add(this.groupDef);
          }
          this.createEvents();
          this.createWatchers();
        }
      },
      immediate: true
    }
  },
  beforeDestroy() {}
};
</script>
