<template>
  <div ref="gantt"></div>
</template>

<script>
import Vue from "vue";
import List from "./List";
import "dhtmlx-gantt";

export default {
  name: "gantt",
  props: {
    tasks: {
      type: Object,
      default() {
        return { data: [], links: [] };
      }
    }
  },
  mounted() {
    gantt.config.columns = [
      { name: "text", label: "Task name", tree: true, width: 120 },
      { name: "start_date", label: "Start time", align: "center" },
      { name: "duration", label: "Duration", align: "center" },
      {
        name: "assignee",
        label: "Assignee",
        align: "center",
        width: 150,
        onrender: (task, node) => {
          Vue.component("List", List);
          new Vue({
            el: node.firstChild,
            render(createElement) {
              return createElement("List", {
                props: {},
                on: {
                  click: function() {
                    gantt.message("Vue component clicked");
                  }
                }
              });
            }
          });
        }
      },
      { name: "add" }
    ];

    gantt.init(this.$refs.gantt);
    gantt.parse(this.$props.tasks);
  }
};
</script>

<style>
@import "~dhtmlx-gantt/codebase/dhtmlxgantt.css";
</style>
