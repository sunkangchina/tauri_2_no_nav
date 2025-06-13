# tauri_2_no_nav

tauri_2 不用默认的navbar,全用vue里面的。

## 安装

~~~
cargo update
npm install -g @tauri-apps/cli
tauri --version
~~~

开发

~~~
tauri dev
~~~

发布

~~~
tauri build
~~~

生成icons

~~~
tauri icon logo.png -o src-tauri/icons
~~~

需要 `1024x1024` 正方形图片

## vue 状态栏
~~~
<div id="title-bar" data-tauri-drag-region>
	<div id="logo">
	  
	</div>

	<div id="window-controls">
	  <div class="window-btn" @click="reloadPage()" title="刷新">↻</div>
	  <div class="window-btn" @click="handleWindowControl('minimize')" title="窗口最小化">—</div>
	  <div class="window-btn" @click="handleWindowControl('maximize')" title="窗口最大化">□</div>
	  <div class="window-btn" @click="handleWindowControl('close')" title="关闭窗口">×</div>
	</div>
</div>



~~~


ts
~~~

import { getCurrentWindow } from '@tauri-apps/api/window';

const currentWindow = getCurrentWindow();

const handleWindowControl = async (action: string) => {
  switch(action) {
    case 'minimize':
      await currentWindow.minimize();
      break;
    case 'maximize':
      await currentWindow.toggleMaximize();
      break;
    case 'close':
      await currentWindow.close();
      break;
    default:
      console.warn('未知操作:', action);
  }
}

~~~


css

~~~
/* 透明状态栏样式 */
#title-bar {
  -webkit-app-region: drag;
  height: 40px; 
  display: flex;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 9999;
  padding-right: 12px;
  background-color: #000;
}

#logo {
  -webkit-app-region: no-drag;
  display: flex;
  align-items: center;
  padding-left: 12px;
  color: white;
  font-weight: bold;
  text-shadow: 0 0 4px rgba(0,0,0,0.5);
}

#logo img {
  height: 24px;
  margin-right: 8px;
}

#window-controls {
  -webkit-app-region: no-drag;
  margin-left: auto;
  display: flex;
}

.window-btn {
  width: 46px;
  height: 40px;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  color: white;
  font-size: 18px;
  font-weight: bold;
}

.window-btn:hover {
  background: rgba(255,255,255,0.2);
}
~~~


## vue选择目录

~~~
import { open } from '@tauri-apps/plugin-dialog';

// 选择目录
const open_local_folder = async () => {
  const selected = await open({
    directory: true,
    multiple: false,
    title: '请选择目录'
  });
  
  if (selected) {
    console.log('选择的目录:', selected); 
  }
}; 
~~~