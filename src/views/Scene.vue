<template>
  <div class="scene-main">
    <content-block caption="场景切换"></content-block>
    <div v-for="scene in scenes" :key="scene.index" class="content" @click="switchScene(scene)">
        <div class="title">{{scene.title}}</div>
        <div class="icon">
          <img src="../assets/icon/scene/scene.png" />
        </div>
    </div>
  </div>
</template>

<script>
  import contentBlock from "../components/content-block.vue";
  import { data, diva } from "../global";

  window.onmessage = (e) => {
    if (e.data && e.data.type) {
      const fun = actions()[e.data.type];
      console.log(e.data.type);
      if (fun) {
        fun.apply(null, e.data.args)
      }
    }
  }

  const actions = () => ({
    switchScene(index) { diva.client.applyScene(index) },
    playCameraTrack(param){diva.client.playCameraTrack(param)},
    //设置时间
    async setTime(time) {
      const date = new Date();
      date.setHours(Number(time), 0, 0, 0);
      await diva.client.setTime(date);
    },
    //设置天气，1：春天，2：夏天，3：秋天，4：冬天
    async setWether(index){
      await diva.client.setSunSimulation(true);
      if(index == 1) await diva.client.setDate(new Date(2025,4,1,12,0,0,0));
      if(index == 2) await diva.client.setDate(new Date(2025,7,1,12,0,0,0));
      if(index == 3) await diva.client.setDate(new Date(2025,10,1,12,0,0,0));
      if(index == 4) await diva.client.setDate(new Date(2025,1,1,12,0,0,0));
    }
  })

  export default {
    data() {
      return {
        scenes: [{
            title: "测试场景01",
            index: 0,
          },
          {
            title: "测试场景02",
            index: 1,
          },
          {
            title: "测试场景03",
            index: 2,
          },
          {
            title: "测试场景04",
            index: 3,
          },
          {
            title: "测试场景05",
            index: 4,
          },
          {
            title: "测试场景06",
            index: 5,
          },
          {
            title: "测试场景07",
            index: 6,
          },
          {
            title: "测试场景08",
            index: 7,
          },
          {
            title: "测试场景09",
            index: 8,
          },
          {
            title: "测试场景10",
            index: 9,
          },
        ],
      }
    },

    methods: {
      switchScene(scene) {
        diva.client.applyScene(scene.index).then(() => {
          data.changeCode(`client.applyScene('${scene.title}')`);
        });
      }
    },

    mounted() {
      diva.client?.applyScene("半鸟瞰").then(() => {
        data.changeCode(`client.applyScene('半鸟瞰')`);
      });
    },
    components: {
      contentBlock
    }
  };
</script>

<style lang="scss">
  .scene-main {
    .content {
      width: 240px;
      height: 60px;
      background: rgba(255, 255, 255, 0.1);
      backdrop-filter: blur(8px);
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 0 20px;
      box-sizing: border-box;
      margin-top: 8px;
      cursor: pointer;
      pointer-events: all;

      .title {
        height: 20px;
        line-height: 20px;
        font-weight: bold;
        color: #fff;
      }

      .icon {
        width: 24px;
        height: 24px;
      }
    }

    .content:hover .title {
      color: #fff;
    }
  }
</style>
