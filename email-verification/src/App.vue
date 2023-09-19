<script setup>
import { ref, watch, computed } from 'vue'
const email = ref('') //TODo ref? 什么时候可以省略value?
const uid = ref('')
const isValid = ref(false)
const inputBorderColor = ref('red')
const DEFAULT_CONNECT_SUFFIX = '@connect.hku.hk'
const DEFAULT_SUFFIX = '@hku.hk'
const MIN_LENGTH = Math.min(
  DEFAULT_CONNECT_SUFFIX.length,
  DEFAULT_SUFFIX.length
)
watch(email, (newInput, oldInput) => {
  var prefix = ''
  if (newInput.length <= MIN_LENGTH) {
    isValid.value = false
  } else if (
    // 后缀检查
    !newInput.endsWith(DEFAULT_CONNECT_SUFFIX) &&
    !newInput.endsWith(DEFAULT_SUFFIX)
  ) {
    isValid.value = false
  } else {
    //前缀检查
    var prefixEndPosition = email.value.indexOf('@') //后缀合法，不用判断prefixEndPosition的边界
    prefix = email.value.substring(0, prefixEndPosition)
    isValid.value = true //假设合法，进行最后一步校验
    for (var i = 0; i < prefix.length; i++) {
      var ascCode = prefix.charCodeAt(i)

      //数字的ascii码 [48, 57], 大写字母的ascii码 [65, 90], 小写字母的ascii码 [97，122]
      if (
        ascCode < 48 ||
        (ascCode > 57 && ascCode < 65) ||
        (ascCode > 90 && ascCode < 97) ||
        ascCode > 122
      ) {
        isValid.value = false
        break
      }
    }
  }
  if (!isValid.value) {
    inputBorderColor.value = 'red'
  } else {
    inputBorderColor.value = 'green' //输入框命中焦点会变红
    uid.value = prefix
  }
})
</script>

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
      <button :disabled="!isValid" id="submit-button">Submit</button>
    </p>
    <p id="id-text">{{ uid }}</p>
  </div>
</template>

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
