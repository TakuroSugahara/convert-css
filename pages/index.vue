<template>
  <div>
    <v-card style="margin-top: 100px;">
      <v-card-title class="headline">
        cssをemotion/coreのcssに変換します
      </v-card-title>
      <v-card-text>
        <v-row>
          <v-col cols="12" md="6">
            <v-textarea
              v-model="code"
              outlined
              hide-details
              auto-grow
              label="css"
            ></v-textarea>
          </v-col>
          <v-col cols="12" md="6">
            <v-textarea
              id="convertedCode"
              outlined
              auto-grow
              hide-details
              readonly
              label="変換後のコード"
              :value="convertedCode"
            ></v-textarea>
          </v-col>
        </v-row>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn :disabled="!code" text @click="copy">
          コピー
          <v-icon right>content_copy</v-icon>
        </v-btn>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      code: '',
    }
  },
  computed: {
    convertedCode() {
      return this.replaceSemiColon(
        this.replaceColon(
          this.replaceChamelCase(this.removeBlankRow(this.code))
        )
      )
    },
  },
  methods: {
    removeBlankRow(originCode) {
      // 空白、空行の削除
      return originCode.replace(/(^[ \t]*\n)/gm, '')
    },
    replaceColon(originCode) {
      // コロンに空白を追加
      return originCode.replace(/:\s?/g, ": '")
    },
    replaceSemiColon(originCode) {
      // セミコロンを改行とコンマに変換
      return originCode.replace(/;/g, "',").replace(/\n$/, '')
    },
    replaceChamelCase(originCode) {
      return originCode.replace(/-./g, (s) => s.charAt(1).toUpperCase())
    },
    copy() {
      const textarea = document.getElementById('convertedCode')
      textarea.select()
      document.execCommand('copy')
      alert('コピーしました！')
    },
  },
}
</script>
