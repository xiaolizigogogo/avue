<template>
  <div class="from-container pull-chheight">
    <Forms :formOption="formOption" v-model="formData" :formSubmitText="formSubmitText" @handleSubmit="handleSubmit">
      <template slot-scope="scope" slot="name">
        <crud-input @click.native="tip" :disabled="scope.column.disabled" v-model="scope.value"></crud-input>
      </template>
    </Forms>
    <el-button @click.native="formate" style="margin: 8px 0">格式化</el-button>
    <el-input type="textarea" :autosize="{ minRows: 2, maxRows: 15}" placeholder="请输入内容" v-model="formJson">
    </el-input>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
import crudInput from "@/components/crud/crud-input";
import crudSelect from "@/components/crud/crud-select";
import crudRadio from "@/components/crud/crud-radio";
import crudCheckbox from "@/components/crud/crud-checkbox";
import crudDate from "@/components/crud/crud-date";
import Forms from "@/components/crud/forms";
import formOption from "@/const/forms/formOption";
export default {
  name: "froms",
  components: {
    Forms,
    crudInput,
    crudSelect,
    crudRadio,
    crudCheckbox,
    crudDate
  },
  data() {
    return {
      formJson: "",
      formOption: formOption,
      formData: {
        username: "smallwei",
        name: "smallwei",
        sex: 0,
        type: 0,
        grade: [0, 1]
      },
      formSubmitText: "确定",
      form: {}
    };
  },
  created() {
    this.formJson = JSON.stringify(formOption, null, 2);
  },
  watch: {},
  mounted() {},
  computed: {
    ...mapGetters(["permission"])
  },
  props: [],
  methods: {
    tip() {
      this.$message({
        message: "点击事件"
      });
    },
    formate() {
      let p = new Promise((resolve, reject) => {
        resolve(JSON.parse(this.formJson));
      });
      p
        .then(data => {
          this.formOption = data;
          this.formJson = JSON.stringify(data, null, 2);
          this.$message({
            message: "数据加载成功",
            type: "success"
          });
        })
        .catch(err => {
          this.$message({
            center: true,
            dangerouslyUseHTMLString: true,
            message: `JSON格式错误<br \>\n${err}`,
            type: "error"
          });
        });
    },
    handleSubmit(form) {
      if (form) {
        this.form = form;
        this.$message({
          message: form,
          type: "success"
        });
      } else {
      }
    }
  }
};
</script>

<style lang="scss" scoped>
.from-container {
  padding: 8px 10px;
}
</style>
