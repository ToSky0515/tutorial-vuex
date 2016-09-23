# vuex����ʵ��(3/3)

���һƪ ��д��vuex 2.0�Ժ�ı仯, ��ʵ�ϸ����岻�������� �����㿴���ͺ� �����ϵ�е����� ����

vuexΪ��ӭ��vue2.0�ı仯 �����˴����ĵ������Ż�

�ȿ����ȴ�2.0����� [������](https://github.com/vuejs/vuex/issues/236)

�ܽ��´���м���仯


## 1. �������廯

ԭ�� Terms naming change for better semantics������廯˵���Ǵ���`action`��`mutation`��API��

��ʹ��`action`��ʱ�� ����һ���Ǵ�vue�������`dispatch`�ɷ�һ��action �����ʵֻ��һ������ ��û��ʵ��
�ı�ʲô�� ��`dispatch`һ��`mutation` ��ʵ�Ǹı���vuex��������� ����һ������ݽǶ���� ����Ӧ�����������ύ����ô�仯֮�����������`commit`�����������廯 Ҳ���õ����ְ��

### �µ�д��
### dispatch --> Action
``` javascript
methods:{
    Add : function(){
      this.$store.dispatch('ADD',2).then(function(resp){
          console.log(resp)
      })
    }
}

```
### commit --> Mutation
``` javascript
actions:{
    "ADD" : function(store , param){
        return new Promise(function(resolve, reject) {
            store.commit('ADD',param)
            resolve("ok");
        })
    }
}
```

1.x д�����Ǵ���`action`��`mutation`����`dispatch`

``` javascript
ADD: function(store, param ){
    store.dispatch('COMMIT',param)
}
```

## 2. �����

1.x֮ǰ�İ汾action�ǲ�������vuex���,  ��2.x actions����ֱ����store�ж����� Ҳ���ǿ�����storeʵ����ֱ��dispatch

```javascript
var store =  new Vuex.Store({
    state: {
        messages: 0
    },
    mutations:{
        "ADD": function(state, msg) {
            state.messages += msg;
        }
    },
    // action������ȥ���涨�� ����ֱ��д�ڹ���������
    actions:{
        "ADD" : function(store , param){
            store.commit('ADD',param)
        },
    }
})
store.dispatch('ADD',2)
```

��getterҲ����� ��vue��ֱ��ȡgetters

```javascript
computed:{
   msg : function(){
      return this.$store.getters.getMessage
   }
}
```


## 3. Promise Action

ԭ�� `Composable Action Flow `ֱ�� `����ϵ�action��`
��ʵ�������¼������� û��ô�ù� Ҳ���÷��� �Ҿͼ򵥴ӱ仯�Է�����
����action���ڷ�����promise ��֮ǰ�İ汾 ��û�з���promise ��2.x��Դ�����Ѿ�������promise
����Ҳ�Ϳ���֧����ν��Composable Action

``` javascript
// action���Ƕ���һ������promise��add action
actions:{
    "ADD" : function(store , param){
        return new Promise(function(resolve, reject) {
            store.commit('ADD',param)
            resolve("ok");
        })
    }
}

// ���������dispatch֮��ֱ�Ӵ����첽
this.$store.dispatch('ADD',2).then(function(resp){
   console.log(resp) // ok
})


```


## 4. MapGetters/ MapActions

�°�vuex�ṩ�˼�����װ���� `mapState`, `mapMutations`, `mapGetters`,`mapActions`

��Щ����ʲô���أ�

��ʵ������ù�vuex1.x���µİ汾 ��ʵ����������vue����е�`vuex`���Ե� ����һ�ָ��߱Ƹ�д��

���Զ���һ��Ҫ��ȡ��actions getters Ȼ��map���� 

```javascript
 // �ɰ�д��
 var App = Vue.extend({
    template:"....",
    vuex:{
        getters:{
            msg : function(state){
                return state.messages
            }
        },
        actions:{
            add :actions.ADD
        }
    }
 })

 // �°�д�� es5 д��
 var App = Vue.extend({
    template:"....",
    computed:Vuex.mapGetters({
        msg : 'getMessage'
    }),
    methods:Vuex.mapGetters({
        add : 'ADD'
    })
})
```
  ���ֱ��ʸ�1.x��`vuex`д����һ���� �ڲ�����ʹ��vue��`Object.defineProperty`ȡ����Ӧʽ
```javascript
// es6д�� ֧��rest��������д�� Ҳ����ֱ����ȫʹ��map��װע��
import { mapGetters, mapActions } from 'vuex'
export default {
  computed: {
    someComuted () { �� },
    ...mapGetters(['getMessage', 'getName'])
  },
  methods: {
    someMethod () { �� },
    ...mapActions(['ADD','EDIT'])
  }
}

```

##5. �����䶯

���µ�`vuex-2.0.0.rc5` Ϊ˵��APIһЩ�±仯
```javascript

   // ������ǻ�������
   store.middlewares -> store.plugins
    
   // ���ò�Ƹɵ��ֱ���ԭ�� �ȴ�����ϲŭ�޳� ��    
   store.watch
   
   // ʹ��subscribe ����vuex�ı仯
   store.subscribe((mutation, state) => { ... })

   // ע��ģ��
   registerModule

   // ע��ģ��
   unregisterModule
```

## �ܽ�

������˵vuex2.0�ı仯���Ǹ�vue����һ�� Ҳ��������һ�� �°��д���ͱƸ����Щ��
��Ϊvueȫ��Ͱ����Ҫ��״̬�������� vuexֵ����ӵ��


