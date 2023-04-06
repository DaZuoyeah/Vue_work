<template>
    <div class="container">
        <div class="form-container">
            <h2 class="title">Login</h2>
            <form>
                <div class="form-group">
                    <label for="name">Name:</label>
                    <input type="name" id="name" v-model="name" required>
                </div>
                <div class="form-group">
                    <label for="password">Password:</label>
                    <input type="password" id="password" v-model="password" required>
                </div>
                <button class="btn btn-primary" type="submit" @click.prevent="login">Login</button>
            </form>
        </div>
    </div>
</template>

<script>

export default {
    
    data() {
        return {
            name: '',
            email: '',
            password: '',

            test_worker_name: '张三',
            test_worker_password: '123456',

            test_manager_name: '李四',
            test_manager_password: '123456'
        }
    },
    methods: {
        login() {
            // Implement login logic here
            console.log('点击了1')
            if((this.test_manager_name == this.name) && (this.test_manager_password == this.password)){
                alert('跳转到老板页面');
                this.$bus.emit('ismanager')
            }

            else if((this.test_worker_name == this.name) && (this.test_worker_password == this.password)){
                alert('跳转到工人页面');
                this.$bus.emit('isworker')
            }

            else{
                if(!this.name || !this.password){
                    if (!this.name) {
                        console.log('点击了2')
                        this.$message({
                            type: 'warning',
                            message: '请输入用户名！！！'
                        })
                    }
                    if (!this.password) {
                        this.$message({
                            type: 'warning',
                            message: '请输入密码！！！'
                        })
                    }
                }else {
                    this.$message({
                        type: 'error',
                        message: '用户名或密码错误！'
                    })
                }

            }

        }
    }
}
</script>

<style scoped>
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.form-container {
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    padding: 20px;
    margin: 10px;
    width: 300px;
}

.title {
    margin-top: 0;
}

.form-group {
    margin-bottom: 10px;
}

label {
    display: block;
    margin-bottom: 5px;
}

input[type="name"],
input[type="password"] {
    width: 100%;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
}

.btn {
    display: inline-block;
    background-color: #4CAF50;
    color: #fff;
    padding: 8px 16px;
    font-size: 14px;
    text-align: center;
    text-decoration: none;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.btn-primary {
    background-color: #008CBA;
}

.btn:hover {
    background-color: #3e8e41;
}

.btn:active {
    background-color: #3e8e41;
    box-shadow: 0 5px #666;
    transform: translateY(4px);
}

.btn:focus {
    outline: none;
}</style>