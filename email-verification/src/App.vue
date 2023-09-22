<template>
  <div class="container">
    <p>
      <input
        type="text"
        v-model="email"
        v-bind:style="{ borderColor: inputBorderColor }"
        id="input-window"
      />
    </p>
    <p>
      <button :disabled="!isValid" id="submit-button" @click="showUid = true">
        Submit
      </button>
    </p>
    <p id="id-text" v-if="showUid && isValid">{{ uid }}</p>
  </div>
</template>

<script>
export const DEFAULT_CONNECT_SUFFIX = '@connect.hku.hk'
export const DEFAULT_SUFFIX = '@hku.hk'
export const MIN_LENGTH = Math.min(
  DEFAULT_CONNECT_SUFFIX.length,
  DEFAULT_SUFFIX.length
)
export default {
  name: 'app',
  data() {
    return {
      email: '',
      uid: '',
      isValid: false,
      showUid: false,
      inputBorderColor: 'red',
    }
  },
  watch: {
    email(newInput, oldInput) {
      var prefix = ''
      if (newInput.length <= MIN_LENGTH) {
        this.isValid = false
      } else if (
        // 后缀检查
        !newInput.endsWith(DEFAULT_CONNECT_SUFFIX) &&
        !newInput.endsWith(DEFAULT_SUFFIX)
      ) {
        this.isValid = false
      } else {
        //前缀检查
        var prefixEndPosition = newInput.indexOf('@') //后缀合法，不用判断prefixEndPosition的边界
        prefix = newInput.substring(0, prefixEndPosition)
        this.isValid = true //假设合法，进行最后一步校验
        for (var i = 0; i < prefix.length; i++) {
          var ascCode = prefix.charCodeAt(i)

          //数字的ascii码 [48, 57], 大写字母的ascii码 [65, 90], 小写字母的ascii码 [97，122]
          if (
            ascCode < 48 ||
            (ascCode > 57 && ascCode < 65) ||
            (ascCode > 90 && ascCode < 97) ||
            ascCode > 122
          ) {
            this.isValid = false
            break
          }
        }
      }
      if (!this.isValid) {
        this.inputBorderColor = 'red'
      } else {
        this.inputBorderColor = 'green'
        this.uid = prefix
      }
    },
  },
}
</script>

<style>
.container {
  width: 300px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
#input-window {
  width: 200px;
  margin: auto 50px;
}
#submit-button {
  width: 100px;
  margin: auto 100px;
}
#id-text {
  width: 200px;
  margin: auto 50px;
  text-align: center;
}
#input-window:focus {
  outline: none;
}

@media (prefers-color-scheme: dark) {
  body {
    background-color: lightpink;
  }
}

@media (prefers-color-scheme: light) {
  body {
    background-color: lightgreen;
  }
}
</style>
