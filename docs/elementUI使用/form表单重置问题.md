#### form表单重置

方法：

> this.$refs[formName].resetFields();

##### 前提：

- 初始化form表单

  - 绑定表单名

    ```vue
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="100px" class="demo-ruleForm">
        <el-form-item label="活动名称" prop="name">
            <el-input v-model="ruleForm.name"></el-input>
          </el-form-item>
    </el-form>
    
    
    data() {
        return {
            ruleForm: {
                name: '',
    		}	
    	},
    }
    ```

    

  - 表单项绑定对应的 `prop` 属性

    ```js
    <el-form-item label="活动名称" prop="name">
        <el-input v-model="ruleForm.name"></el-input>
    </el-form-item>
    ```

    