<template>
    <div class="app-container">
        <!-- 输入表单 -->
        <el-form label-width="120px">
            <el-form-item label="借款额度">
                <el-input-number v-model="integralGrade.borrowAmount" :min="0" />
            </el-form-item>
            <el-form-item label="积分区间开始">
                <el-input-number v-model="integralGrade.integralStart" :min="0" />
            </el-form-item>
            <el-form-item label="积分区间结束">
                <el-input-number v-model="integralGrade.integralEnd" :min="0" />
            </el-form-item>
            <el-form-item>
                <el-button :disabled="saveBtnDisabled" type="primary" @click="saveOrUpdate()">
                    保存
                </el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script>
//引入api模块
import integralGradeApi from '@/api/core/integral-grade';

export default {
    data() {
        return {
            integralGrade: {}, // 初始化数据
            saveBtnDisabled: false // 保存按钮是否禁用，防止表单重复提交
        }
    },


    //页面渲染成功
    created() {
        if (this.$route.params.id) {
            this.fetchDataById(this.$route.params.id)
        }
    },

    methods: {
        // fetchById(id) {
        //     integralGradeApi.getById(id).then(response => {
        //         this.integralGrade = response.data.record
        //     })
        // },

        // 根据id查询记录
        fetchDataById(id) {
            integralGradeApi.getById(id).then(response => {
                this.integralGrade = response.data.record
            })
        },


        //保存或更新
        saveOrUpdate() {
            // 禁用保存按钮
            this.saveBtnDisabled = true
            if (!this.integralGrade.id) {
                this.saveData()
            } else {
                this.updateData()
            }
        },

        // 新增数据
        saveData() {
            // debugger
            integralGradeApi.save(this.integralGrade).then(response => {
                this.$message({
                    type: 'success',
                    message: response.message
                })
                this.$router.push('/core/integral-grade/list')//改变保存后的路由
            })
        },

        updateData() {
            // 数据的获取
            integralGradeApi.updateById(this.integralGrade).then(response => {
                this.$message({
                    type: 'success',
                    message: response.message
                })
                this.$router.push('/core/integral-grade/list')
            })
        }
    }
}

</script>