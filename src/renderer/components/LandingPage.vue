<template>
  <div>
    <div v-if="processing" style="width: 100%;height: 100%;opacity:0.5;position: absolute;z-index: 100000;background-color: gray">
      <div style="position: absolute; bottom: 0px">
        <div  v-for="(item,i) in data.message">{{item}}</div>
        处理中 ...
      </div>
    </div>
    <div id="wrapper">

      <!--<img id="logo" src="~@/assets/logo.png" alt="electron-vue">-->
      <main>

        <div class="left-side">
        <span class="title">
          欢迎使用ElasticSearch Dump UI
        </span>
          <!--<system-information></system-information>-->
        </div>

        <div class="right-side doc">
          <div><label>源地址</label><input type="text" v-model="options.input" placeholder="请输入原地址"></div>
          <div><label>源索引</label><input type="text" v-model="options['input-index']" placeholder="请输入原索引，如index/type"></div>
          <div><label>类型</label>
            <select v-model="options.type">
              <option v-for="(item,i) in types" :value="item">{{item}}</option>
            </select>
            <!--<template v-for="(item,i) in types" ><input class="radio" type="radio" v-model="options.type" :value="item">{{item}} </template>-->
          </div>
          <div><label>目标地址</label><input type="text" v-model="options.output" placeholder="请输入目标地址"></div>
          <div><label>目标索引</label><input type="text" v-model="options['output-index']" placeholder="请输入目标索引，如index/type"></div>
          <div><label>拉取限制</label><input type="number" v-model="options.limit"></div>
          <div><button class="button" @click="open('https://www.baidu.com')">运行</button></div>
        </div>
      </main>


    </div>
  </div>

</template>

<script>
  import Elasticdump from 'elasticdump/elasticdump.js'
  export default {
    data () {
      return {
        processing: false,
        data: {
          message: []
        },
        types: ['settings', 'analyzer', 'data', 'mapping', 'alias', 'template'],
        options: {
          size: -1,
          limit: 1000,
          offset: 0,
          debug: false,
          type: 'data',
          delete: false,
          maxSockets: null,
          input: 'http://localhost:9200',
          'input-index': '',
          output: 'output.json',
          'output-index': '',
          noRefresh: false,
          inputTransport: null,
          outputTransport: null,
          searchBody: null,
          searchWithTemplate: false,
          headers: null,
          sourceOnly: false,
          jsonLines: false,
          format: '',
          'ignore-errors': false,
          'support-big-int': false,
          scrollTime: '10m',
          timeout: null,
          toLog: null,
          quiet: false,
          awsChain: false,
          awsAccessKeyId: null,
          awsSecretAccessKey: null,
          awsIniFileProfile: null,
          awsService: null,
          awsRegion: null,
          s3AccessKeyId: null,
          s3SecretAccessKey: null,
          s3Region: null,
          s3Endpoint: null,
          s3SSLEnabled: true,
          s3ForcePathStyle: false,
          s3Compress: false,
          fsCompress: false,
          awsIniFileName: null,
          sessionToken: null,
          transform: null,
          httpAuthFile: null,
          params: null,
          prefix: '',
          suffix: '',
          retryAttempts: 0,
          customBackoff: false,
          retryDelayBase: 0,
          retryDelay: 5000,
          parseExtraFields: '',
          fileSize: -1,
          cert: null,
          key: null,
          pass: null,
          ca: null,
          tlsAuth: false,
          'input-cert': null,
          'input-key': null,
          'input-pass': null,
          'input-ca': null,
          'output-cert': null,
          'output-key': null,
          'output-pass': null,
          'output-ca': null,
          inputSocksProxy: null,
          inputSocksPort: null,
          outputSocksProxy: null,
          outputSocksPort: null,
          concurrency: 1,
          throttleInterval: 1,
          carryoverConcurrencyCount: true,
          intervalCap: 5,
          concurrencyInterval: 5000,
          overwrite: true,
          handleVersion: false,
          versionType: null
        }
      }
    },
    name: 'landing-page',
    methods: {
      open (link) {
        this.processing = true
        let that = this
        this.data.message = []
        let dumper = new Elasticdump(this.options.input, this.options.output, this.options)
        dumper.on('log', function (message) {
          console.log(message)
          // that.message = message
          that.data.message.push(message)
          if (that.data.message.length > 2) {
            that.data.message.splice(0, 1)
          }
        })
        dumper.on('debug', function (message) { console.debug(message) })
        dumper.on('error', function (error) { alert(`Error Emitted => ${error.message || JSON.stringify(error)}`) })
        dumper.dump(function (error) {
          if (error) {
            console.error(error)
            alert(error)
            that.processing = false
          } else {
            alert('处理结束')
            that.processing = false
          }
        })
      }
    }
  }
</script>

<style>
  /*@import url('https://fonts.googleapis.com/css?family=Source+Sans+Pro');*/

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body { font-family: 'Source Sans Pro', sans-serif; }

  #wrapper {
    background:
      radial-gradient(
        ellipse at top left,
        rgba(255, 255, 255, 1) 40%,
        rgba(229, 229, 229, .9) 100%
      );
    height: 100vh;
    padding: 60px 80px;
    width: 100vw;
  }

  #logo {
    height: auto;
    margin-bottom: 20px;
    width: 420px;
  }

  main {
    /*display: flex;*/
    justify-content: space-between;
  }

  /*main > div { flex-basis: 50%; }*/

  .left-side {
    display: flex;
    flex-direction: column;
  }

  .welcome {
    color: #555;
    font-size: 23px;
    margin-bottom: 10px;
  }

  .title {
    color: #2c3e50;
    font-size: 20px;
    font-weight: bold;
    margin-bottom: 6px;
  }

  .title.alt {
    font-size: 18px;
    margin-bottom: 10px;
  }

  .doc div {
    color: black;
    margin-bottom: 10px;
  }

  .doc button {
    font-size: .8em;
    cursor: pointer;
    outline: none;
    padding: 0.75em 2em;
    border-radius: 2em;
    display: inline-block;
    color: #fff;
    background-color: #4fc08d;
    transition: all 0.15s ease;
    box-sizing: border-box;
    border: 1px solid #4fc08d;
  }

  .doc button.alt {
    color: #42b983;
    background-color: transparent;
  }
  .doc label{
    min-width: 100px;
    display: inline-block;
    /*font-size: 12px;*/
  }
  .radio {
    display: inline-block;
    margin-right: 5px;
  }
  input {
    height: 20px;
    min-width: 300px;
  }
</style>
