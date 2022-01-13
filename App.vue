<template>
  <div id="q-app" class="app" :class="{'app_loading':!ready}">
    <router-view v-if="ready"/>

    <AlertWindow/>
  </div>
</template>

<script>
import { mapGetters, mapActions, mapMutations } from 'vuex'
import AlertWindow from "components/Alert.vue"

export default {
  name: 'App',
  data(){
      return {
          ws: {
              connection: null,
              server: process.env.WSS,
              countErrorConnect: 0
          }
      }
  },
  created(){
      const vm = this;
      this.init()
          .then(() => {
              if(!vm.isAuth && !['Login', 'Signin'].includes(vm.$route.name)) vm.$router.replace({name: 'Login'});
              if(vm.isAuth) {
                  vm.connect();
                  if(vm.$route.name !== 'Dialogs') vm.$router.replace({name: 'Dialogs'});
              }
          })
          .catch(() => {
              vm.openWindow({
                  name: 'alert',
                  props: {
                      message: vm.$t('server.error')
                  }
              })
          });
  },
  components: {
      AlertWindow,
  },
  computed: {
    ...mapGetters({
        ready: 'app/ready',
        isAuth: 'app/isAuth',
        isWindowExist: 'popup/isWindowExist',
        accountData: 'app/accountData',
    }),
  },
  methods: {
    ...mapMutations({
        openWindow: 'popup/open',
        closeWindow: 'popup/close',
        UpdateUsersStatus: 'app/UpdateUsersStatus',
        removeFromDisk: 'app/removeFromDisk',
        closeNotify: 'dialog/closeNotify',
        setWSConnection: 'app/setWSConnection',
        removeWSConnection: 'app/removeWSConnection',
        setEmployeeToDialog: 'dialog/setEmployeeToDialog',
        removeDialogMutation: 'dialog/remove',
        removeDialogsMutation: 'dialog/removeList',
        EmployeeCancelDialog: 'dialog/cancel',
        MoveDialogToArchive: 'dialog/archive',
        MoveDialogToDialogs: 'dialog/unarchive',
        PassDialogToEmployee: 'dialog/passToEmployee',
        addNotify: 'dialog/addNotify',
        blockDialog: 'dialog/block',
        unblockDialog: 'dialog/unblock',
        removeArchive: 'dialog/removeArchive',
    }),
    ...mapActions({
        init: 'app/initialize',
        FetchNewAccount: 'app/FetchNewAccount',
        UpdateAccount: 'app/UpdateAccount',
        FetchNewSite: 'app/FetchNewSite',
        UpdateSite: 'app/UpdateSite',
        ReceiveNewMessage: 'dialog/ReceiveNewMessage',
        TypingMessage: 'dialog/TypingMessage',
        DeleteAccountRelations: 'app/deleteAccountRelations',
    }),
    connect(){
        const vm = this;
        if(!vm.ws.connection){
            let token = localStorage.getItem('token');

            let socket = new WebSocket(`${vm.ws.server}/?u=${token}`);
            socket.onopen = () => {
                console.log('[socket] open');
                vm.ws.connection = socket;
                vm.setWSConnection(socket);
            };
            socket.onclose = () => {
                console.log('[socket] close');
                vm.ws.connection = null;
                vm.removeWSConnection();
                vm.ws.countErrorConnect += 1;
                setTimeout(() => vm.connect(), 2000);
            };
            socket.onerror = e => {
                console.log('[socket] error', e);
            };
            socket.onmessage = e => {
                if(e.data){
                    try{
                        let data = JSON.parse(e.data);
                        if(data) vm.newMessage(data);
                    }catch (e) {
                        console.log('[socket message] Parse message error', e);
                    }
                }
            };
        }
    },
    newMessage(message){
        console.log('[socket message]', message);
        const vm = this;
        if(message.type === 'UPD_user_status') vm.UpdateUsersStatus(message.active_users);
        else if(message.type === 'CreateAccount') vm.FetchNewAccount(message.accountID);
        else if(message.type === 'UpdateAccount') vm.UpdateAccount(message.accountID);
        else if(message.type === 'DeleteAccount') {
            if(message.accountID === vm.accountData.id) window.location.reload();
            else{
                let employeeWindow = vm.isWindowExist('employee');
                if(employeeWindow && employeeWindow.id === message.accountID){
                    vm.closeWindow('employee');
                    let employeeInviteWindow = vm.isWindowExist('employeeInvite');
                    if(employeeInviteWindow) vm.closeWindow('employeeInvite');
                    vm.openWindow({name: 'alert', props: {message: vm.$t('server.errors.EmployeeWasDeleted')}});
                }
                vm.DeleteAccountRelations(message.accountID);
            }
        }
        else if(message.type === 'CreateSite') vm.FetchNewSite(message.siteID);
        else if(message.type === 'UpdateSite') vm.UpdateSite(message.siteID);
        else if(message.type === 'NewMessage') {
            vm.ReceiveNewMessage(message);
            setTimeout(() => vm.closeNotify(message), 5000);
            //vm.soundNotify();
        }
        else if(message.type === 'Typing') vm.TypingMessage(message.message);
        else if(message.type === 'SetEmployeeToDialog') vm.setEmployeeToDialog(message.message);
        else if(message.type === 'RemoveDialog') {
            let dialogWindow = vm.isWindowExist('dialog');
            if(dialogWindow && dialogWindow.id === message.message.id){
                vm.closeWindow('dialog');
                vm.openWindow({name: 'alert', props: {message: vm.$t('server.errors.DialogWasDeleted')}});
            }
            vm.removeDialogMutation(message.message.id);
            vm.removeFromDisk(message.message.free);
        }
        else if(message.type === 'RemoveDialogs') {
            let dialogWindow = vm.isWindowExist('dialog');
            if(dialogWindow && message.message.dialogs.includes(dialogWindow.id)){
                vm.closeWindow('dialog');
                vm.openWindow({name: 'alert', props: {message: vm.$t('server.errors.DialogWasDeleted')}});
            }
            vm.removeDialogsMutation(message.message.dialogs);
            vm.removeFromDisk(message.message.free);
        }
        else if(message.type === 'CancelDialog') vm.EmployeeCancelDialog(message.dialog);
        else if(message.type === 'ArchiveDialog') {
            let dialogWindow = vm.isWindowExist('dialog');
            if(dialogWindow && dialogWindow.id === message.dialog){
                vm.closeWindow('dialog');
                vm.openWindow({name: 'alert', props: {message: vm.$t('dialogs.movedToArchive')}});
            }
            vm.MoveDialogToArchive(message.dialog);
        }
        else if(message.type === 'UnArchiveDialog') {
            let dialogWindow = vm.isWindowExist('dialog');
            if(dialogWindow && dialogWindow.id === message.dialog){
                vm.closeWindow('dialog');
                vm.openWindow({name: 'alert', props: {message: vm.$t('dialogs.movedToDialogs')}});
            }
            vm.MoveDialogToDialogs(message.dialog);
        }
        else if(message.type === 'PassDialog') {
            vm.PassDialogToEmployee({dialogID: message.dialog, employeeID: message.employee});
            if(vm.accountData.id === message.employee){
                vm.addNotify({
                    type: message.type,
                    header: 'PassDialog',
                    message: vm.$t('notifications.PassDialogDesc'),
                    addition: {
                        dialogID: message.dialog
                    }
                });
                setTimeout(() => vm.closeNotify({type: message.type, addition: {dialogID: message.dialog}}), 5000);
            }
        }
        else if(message.type === 'BlockDialog') {
            vm.blockDialog(message.dialog);
            let dialogWindow = vm.isWindowExist('dialog');
            if(dialogWindow && dialogWindow.id === message.dialog){
                vm.openWindow({name: 'alert', props: {message: vm.$t('dialogs.blockDialogSuccess')}});
            }
        }
        else if(message.type === 'UnBlockDialog') {
            vm.unblockDialog(message.dialog);
            let dialogWindow = vm.isWindowExist('dialog');
            if(dialogWindow && dialogWindow.id === message.dialog){
                vm.openWindow({name: 'alert', props: {message: vm.$t('dialogs.unblockDialogSuccess')}});
            }
        }
        else if(message.type === 'RemoveArchive') {
            vm.removeArchive(message.site);
        }
    },
    soundNotify(){
        const audio = new Audio(process.env.ChatSupportURL + process.env.notificationSoundFile);
        audio.play();
    }
  }
}
</script>
