<template>
  <div class="row justify-start items-center">
    <div
      v-for="(a, i) in getAllAuth"
      :key="`auth${i}`"
      class="round-borders q-body-1 q-pa-sm q-ma-xs bg-bg1 row items-center animate-pop"
    >
      <q-icon name="mdi-shield-account" class=" text-text2" size="16px" />
      <div class="q-caption q-mx-sm">{{ `${a.actor}@${a.permission}` }}</div>
      <q-btn
        v-if="i >= auth.length"
        icon="close"
        size="xs"
        color="negative"
        flat
        dense
        @click="deleteAuth(i)"
      />
    </div>

    <div class="row">
      <q-btn icon="add" flat dense round @click="addAuth(new_auth)" />
      <q-input
        :dark="getIsDark"
        color="primary-light"
        placeholder="actor@permission"
        style="margin-left:5px"
        v-model="new_auth"
        @keyup.enter.native="addAuth(new_auth)"
      />
    </div>
  </div>
</template>

<script>
import { mapGetters } from 'vuex';
export default {
  // name: 'ComponentName',
  props: {
    auth: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      auths: [],
      new_auth: ''
    };
  },
  computed: {
    ...mapGetters({
      getAccount: 'user/getAccount',
      getIsDark: 'ui/getIsDark'
    }),
    getAllAuth() {
      if (this.auth.length == 0 && this.getAccount && this.auths.length == 0) {
        this.auths.push({
          actor: this.getAccount.name,
          permission: this.getAccount.authority
        });
      }
      let all = [...this.auth, ...this.auths];
      this.$emit('input', JSON.stringify(all));
      return all;
    }
  },
  methods: {
    addAuth(authstring) {
      if (authstring == '') {
        return;
      }
      let [actor, perm] = authstring.split('@');
      if (!actor || !perm) {
        this.new_auth = '';
        return;
      }
      this.auths.push({ actor: actor, permission: perm });
      this.new_auth = '';
    },
    deleteAuth(index) {
      index = index - this.auth.length;
      console.log(index);
      this.auths.splice(index, 1);
    }
  }
};
</script>

<style></style>
