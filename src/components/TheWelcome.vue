<template>
  <div id="drag">
    <div id="content" />
</div>
</template>
<script>
  const MAX_HEIGHT = 700;
  const MIN_HGIEHT = 100;
  const WINDOW_HEIGHT = window.innerHeight;
  export default {
    data() {
      return {
        isResizing: false,
        currentResizer: null
      }
    },
    methods: {},
    created() {
      this.$nextTick(() => {
        const el = document.querySelector("#drag");
        this.isResizing = false;
        const resizers = document.querySelectorAll("#content");
        for (let resizer of resizers) {
          resizer.addEventListener("mousedown", mousedown);

          function mousedown(e) {
            this.currentResizer = e.target;
            this.isResizing = true;
            let prevY = e.clientY;
            window.addEventListener("mousemove", mousemove);
            window.addEventListener("mouseup", mouseup);

            function mousemove(e) {
              const rect = el.getBoundingClientRect();
              const currentHeight = rect.height + (prevY - e.clientY);
              const currenTop = rect.top - (prevY - e.clientY);
              
              if (currentHeight >= MAX_HEIGHT) {
                el.style.height = MAX_HEIGHT + "px";
                el.style.top = WINDOW_HEIGHT - MAX_HEIGHT + "px";
              } else if (currentHeight <= MIN_HGIEHT) {
                el.style.height = MIN_HGIEHT + "px";
                el.style.top = WINDOW_HEIGHT - MIN_HGIEHT + "px";
              } else {
                el.style.height = currentHeight + "px";
                el.style.top = currenTop + "px";
              }
              prevY = e.clientY;
            }

            function mouseup() {
              window.removeEventListener("mousemove", mousemove);
              window.removeEventListener("mouseup", mouseup);
              this.isResizing = false;
            }
          }
        }
      })
    },
    
  }
</script>
<style scoped>
#drag {
  width: 100%;
  height: 50%;
  position: absolute;
  background: orange;
  bottom: 0;
  left: 0;
}

#content {
  position: fixed;
  left: 0;
  width: 100%;
  height: 5px;
  background-color: black;
  z-index: 2;
  cursor: n-resize;
  max-height: 600px;
}
</style>
