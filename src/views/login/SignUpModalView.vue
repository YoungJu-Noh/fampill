<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on, attrs }">
      <v-btn :color="btnColor" light v-bind="attrs" v-on="on">
        <slot name="title"></slot>
      </v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5"
          ><v-icon>mdi-account</v-icon>회원 가입 Sample</span
        >
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <v-col cols="12">
              <v-text-field
                label="아이디*"
                required
                v-model="user.id"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                label="비밀번호*"
                type="password"
                required
                v-model="user.pwd"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                ref="checkPwd"
                label="비밀번호 확인*"
                type="password"
                required
                v-model="user.checkPwd"
              ></v-text-field>
            </v-col>
            <v-col cols="12">
              <v-text-field
                label="이름*"
                required
                v-model="user.name"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="dialog = false"> 취소 </v-btn>
        <v-btn color="blue darken-2" text @click="submit"> 가입 </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { callSignUp } from "@/service/auth";

export default {
  props: {
    btnColor: {
      type: String,
      default: "primary",
    },
  },

  data: () => ({
    dialog: false,
    user: {
      id: "",
      pwd: "",
      checkPwd: "",
      name: "",
    },
  }),

  methods: {
    async submit() {
      try {
        const response = await callSignUp({
          ...this.user,
        });

        if (response.status === 201) {
          alert("회원가입 완료");
          this.dialog = false;
        }
      } catch (error) {
        console.error(error);
        alert("네트워크 장애");
      }
    },
  },
};
</script>

<style scoped></style>
