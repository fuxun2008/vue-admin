<template>
  <div class="login">
    <div id="particles-background" class="background-canvas"></div>
    <div class="content">
      <div class="title">管理登录</div>
      <el-form :model="form" ref="form">
        <el-form-item 
          prop="username"
          :rules="[
            { required: true, message: '用户名', trigger: 'blur' },
            { min: 5, message: '用户名至少6位', trigger: 'blur' }
          ]">
          <el-input placeholder="账号" v-model="form.username" :maxlength="40"></el-input>
        </el-form-item>
        <el-form-item 
          prop="password"
          :rules="[
            { required: true, message: '密码', trigger: 'blur' },
            { min: 6, message: '密码至少6位', trigger: 'blur' }
          ]">
          <el-input 
            placeholder="密码" 
            v-model="form.password" 
            :maxlength="40" 
            type="password"
            @keyup.enter.native="submit('form')"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button @click.native="submit('form')" :disabled="logining">{{ logining ? 'login...' : 'Submit'}}</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script lang="ts">

import Vue from 'vue'
import Component from 'vue-class-component'
import 'particles.js'

@Component
export default class login extends Vue {
  $refs: {
    form: HTMLFormElement
  }

  private form: StoreState.Login = {
    username: '',
    password: ''
  }

  private get logining (): boolean {
    return this.$store.state.login
  }

  // 提交表单
  private submit (): void {
    this.$refs.form.validate(async(valid: boolean): Promise<boolean> => {
      if (valid) {
        const data: Ajax.AjaxResponse = await this.$store.dispatch('login', { ...this.form })
        if (data.code !== 1) return false
        if (!this.$route.query.redirect) this.$router.push('/home')
        else this.$router.push(this.$route.query.redirect)
        return true
      } else {
        return false
      }
    })
  }

  public buildBackground (): void {
    particlesJS ('particles-background', {
      'particles': {
        'number': {
          'value': 30,
          'density': {
            'enable': true,
            'value_area': 1000
          }
        },
        'color': {
          'value': '#bebebe'
        },
        'shape': {
          'type': 'circle',
          'stroke': {
            'width': 0,
            'color': '#ffffff'
          },
          'polygon': {
            'nb_sides': 5
          },
          'image': {
            'src': 'img/github.svg',
            'width': 100,
            'height': 100
          }
        },
        'opacity': {
          'value': 0.3
        },
        'size': {
          'value': 15,
          'random': true,
          'anim': {
            'enable': false,
            'speed': 40,
            'size_min': 0.1,
            'sync': false
          }
        },
        'line_linked': {
          'enable': true,
          'distance': 300,
          'color': '#c5c5c5',
          'opacity': 0.4,
          'width': 1
        },
        'move': {
          'enable': true,
          'speed': .5,
          'direction': 'none',
          'random': true,
          'straight': false,
          'out_mode': 'out',
          'bounce': false,
          'attract': {
            'enable': false,
            'rotateX': 600,
            'rotateY': 1200
          }
        }
      },
      'interactivity': {
        'detect_on': 'canvas',
        'events': {
          'onhover': {
            'enable': false,
            'mode': 'repulse'
          },
          'onclick': {
            'enable': false,
            'mode': 'push'
          },
          'resize': true
        },
        'modes': {
          'grab': {
            'distance': 400,
            'line_linked': {
              'opacity': 1
            }
          },
          'bubble': {
            'distance': 400,
            'size': 40,
            'duration': 2,
            'opacity': 0.8471528471528471,
            'speed': 3
          },
          'repulse': {
            'distance': 200,
            'duration': 0.4
          },
          'push': {
            'particles_nb': 4
          },
          'remove': {
            'particles_nb': 2
          }
        }
      },
      'retina_detect': true
    })
  }

  mounted () {
    setTimeout(() => {
      this.buildBackground()
    }, 0)
  }
}
</script>

<style lang="scss">

@import '../assets/scss/variable.scss';

.login {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
  background: $mobile-bg;

  >.background-canvas {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }

  >.content {
    position: absolute;
    width: 300px;
    height: 500px;
    z-index: 999;

    >.title {
      font-size: $font-size-logo * 1.5;
      text-align: center;
    }

    >.el-form {
      margin-top: 16px;

      .el-input__inner {
        height: $xlg-pad;
      }

      .el-button {
        display: block;
        height: $xlg-pad;
        width: 100%;
      }
    }
  }
}
</style>
