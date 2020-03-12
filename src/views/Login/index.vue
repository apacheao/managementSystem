<template>
  <div id="login">
    <div class="login-wrap">
      <ul class="menu-tab">
        <li
          :class="{ current: item.current }"
          @toggleMenu="toggleMenu(item)"
          v-for="item in menuTab"
          :key="item.id"
        >
          {{ item.txt }}
        </li>
      </ul>
      <el-form
        :model="ruleForm"
        status-icon
        :rules="rules"
        ref="ruleForm"
        class="login-form"
      >
        <el-form-item prop="userEmail">
          <label>邮箱</label>
          <el-input
            type="text"
            v-model="ruleForm.userEmail"
            autocomplete="off"
            class="item-form"
          ></el-input>
        </el-form-item>
        <el-form-item prop="pass">
          <label>密码</label>
          <el-input
            type="password"
            minlength="6"
            maxlength="22"
            v-model="ruleForm.pass"
            autocomplete="off"
            class="item-form"
          ></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <label>验证码</label>
          <el-row :gutter="20">
            <el-col :span="15">
              <el-input
                v-model.number="ruleForm.code"
                class="item-form"
                minlength="6"
                maxlength="6"
              ></el-input>
            </el-col>
            <el-col :span="9">
              <el-button type="success" class="block">获取验证码</el-button>
            </el-col>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button
            type="danger"
            @click="submitForm('ruleForm')"
            class="login-btn block"
            >提交</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { stripscript, validateEmail, validatePass, validateVCode } from '@/utils/validate'
export default {
  name: "index",
  data() {
    // 检测邮箱
    let validateUserEmail = (rule, value, callback) => {
      if (value === "") {
        callback(new Error("邮箱不能为空"));
      } else if (!validateEmail(value)) {
        callback(new Error("邮箱格式有误"));
      } else {
        callback();
      }
    };
    // 验证密码
    let validatePassword = (rule, value, callback) => {
      this.ruleForm.pass = stripscript(value)
      value = this.ruleForm.pass
      if (value === "") {
        callback(new Error("密码不能为空"));
      } else if (!validatePass(value)) {
        callback(new Error("请输入6至20位的字母+数字的密码"));
      } else {
        callback();
      }
    };
    // 验证验证码
    let validateCode = (rule, value, callback) => {
      this.ruleForm.code = stripscript(value)
      value = this.ruleForm.code
      if (value === "") {
        return callback(new Error("验证码不能为空"));
      } else if (!validateVCode(value)) {
        callback(new Error("请输入6的验证码"));
      } else {
        callback()
      }
    };
    return {
      menuTab: [
        { txt: "登录", current: false },
        { txt: "注册", current: true }
      ],
      ruleForm: {
        userEmail: "",
        pass: "",
        code: ""
      },
      rules: {
        userEmail: [{ validator: validateUserEmail, trigger: "blur" }],
        pass: [{ validator: validatePassword, trigger: "blur" }],
        code: [{ validator: validateCode, trigger: "blur" }]
      }
    };
  },
  methods: {
    toggleMenu(data) {
      this.menuTab.forEach(item => {
        item.current = false;
      });
      data.current = true;
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          alert("submit!");
        } else {
          console.log("error submit!!");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="stylus" scoped>
#login {
    height: 100vh;
    background-color: #344a5f;

    .login-wrap {
        width: 330px;
        margin: auto;

        .menu-tab {
            text-align: center;

            li {
                display: inline-block;
                width: 88px;
                line-height: 36px;
                font-size: 14px;
                color: #fff;
                border-radius: 2px;
                cursor: pointer;
            }

            .current {
                background-color: rgba(0, 0, 0, .1);
            }
        }

        .login-form {
            margin-top 29px

            label {
                display: block;
                margin-bottom: 3px;
                font-size: 14px;
                color: #fff;
            }

            .item-form {
                margin-bottom: 13px;
            }

            .block {
                width: 100%;
                display block
            }

            .login-btn {
                margin-top 19px
            }
        }
    }
}
</style>
