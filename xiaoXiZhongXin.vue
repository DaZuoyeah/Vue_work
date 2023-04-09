<template>
    <v-card class="mx-auto" max-width="450">
        <v-toolbar color="cyan-lighten-1">
            <v-btn variant="text" icon="mdi-menu"></v-btn>

            <v-toolbar-title>消息中心</v-toolbar-title>

            <v-spacer></v-spacer>

            <v-btn variant="text" icon="mdi-magnify"></v-btn>
        </v-toolbar>

        <v-list v-for='item,index in items' :key='index' item-props lines="three">
            <div @click="infoTipIcon(index, item.subtitle)">
                <div class="info" v-html="item.subtitle"></div>  
                <div>
                    <div class="unreadResumeWarning" v-if="1 != this.flag[index]"></div>
                    <div class="divide"></div>
                </div>
                
            </div> 
        </v-list>
    </v-card>

    <dialog-component v-if="Visiable" ref="dialog"></dialog-component>

</template>


<script>
import EventEmitter from 'events';
import dialogComponent from './dialogComponent.vue';


export default {
    name: 'xiaoXiZhongXin',
    components:{
        dialogComponent,
    },
    data: () => ({
        Visiable: false,
        items: [
            {
                title: 'Brunch this weekend?',
                subtitle: `<span class="text-primary">Ali Connors</span> &mdash; I'll be in your neighborhood doing errands this weekend. Do you want to hang out?`,
            },
            
            {
                title: 'Summer BBQ',
                subtitle: `<span class="text-primary">to Alex, Scott, Jennifer</span> &mdash; Wish I could come, but I'm out of town this weekend.`,
            },
            
            {
                title: 'Oui oui',
                subtitle: '<span class="text-primary">Sandra Adams</span> &mdash; Do you have Paris recommendations? Have you ever been?',
            },
            
            {
                title: 'Birthday gift',
                subtitle: '<span class="text-primary">Trevor Hansen</span> &mdash; Have any ideas about what we should get Heidi for her birthday?',
            },
            
            {
                title: 'Recipe to try',
                subtitle: '<span class="text-primary">Britta Holt</span> &mdash; We should eat this: Grate, Squash, Corn, and tomatillo Tacos.',
            },
        ],

        message:[],

        flag: [],
    }),

    methods: {
        infoTipIcon: function(index,info){
            this.flag[index] = 1;
            this.Visiable = true;
            this.$nextTick(() => {
                //这里的dialog与上面dialog-component组件里面的ref属性值是一致的
                //init调用的是dialog-component组件里面的init方法
                //data是传递给弹窗页面的值
                this.$refs.dialog.init(info);
            })
        }
    },

    setup(){
        // 添加事件监视器
        let emitter = new EventEmitter();
        // 说明根据传递过来的消息然后转化到item里显示在template中，其中默认每个消息的flag初始为0
        emitter.on('sendMessage',(message)=>{
            this.message = message;
        });
    },
}
</script>

<style>
    .unreadResumeWarning{
        /* float: left; */
        margin-left: auto;
        width: 10px;
        height: 10px;
        border-radius: 7px;
        background-color: #FF3B30;
    }

    .divide{
        height:1px;
        border:none;
        border-top:1px 
        dashed #c9d9e8;
    }




</style>