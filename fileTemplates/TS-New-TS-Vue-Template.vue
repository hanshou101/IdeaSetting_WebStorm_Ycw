<template>
    <section class="${dash_name}">
    </section>
</template>

<script lang="ts">
  import ${NAME} from "./${dash_name}";
  export default ${NAME};

  @MyComponent({
    name: "${NAME}",
    components: {},
    filters: {}
  })
  export default class ${NAME} extends AbstractBaseVueClass_NotDirectExtendedByMixins {
    // 混入在此处，进行添加。
 
    // Data，在类中的实现 （双向绑定除外）
 
    // Method，在类中的实现
 
    // Lifecycle生命周期，在类中的实现
    created (): void {
    }
 
    mounted (): void {
    }
 
    activated (): void {
    }
 
    updated (): void {
    }
 
    destroyed (): void {
    }
    
    public MixinsData_1: Mixins_LevelTagCompanion_Widget = {} as any;

  }  

</script>

<style scoped lang="stylus">
  @import './${dash_name}.scoped.styl';
  
   .${dash_name} {
 
   }

</style>
