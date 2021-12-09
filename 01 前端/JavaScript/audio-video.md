### audio 标签

不支持 audio 标签的浏览器会显示文本内容

```html
<audio src="sidebyside.mp3" controls="controls">
        当前浏览器不支持 audio
 </audio>
```

#### 属性

| 属性                                                         | 值       | 描述                                                         | 备注               |
| :----------------------------------------------------------- | :------- | :----------------------------------------------------------- | ------------------ |
| [autoplay](https://www.w3school.com.cn/tags/att_audio_autoplay.asp) | autoplay | 如果出现该属性，则音频在就绪后马上播放。                     | 现在不允许自动播放 |
| [controls](https://www.w3school.com.cn/tags/att_audio_controls.asp) | controls | 如果出现该属性，则向用户显示控件，比如播放按钮。             |                    |
| [loop](https://www.w3school.com.cn/tags/att_audio_loop.asp)  | loop     | 如果出现该属性，则每当音频结束时重新开始播放。               |                    |
| [muted](https://www.w3school.com.cn/tags/att_audio_muted.asp) | muted    | 规定视频输出应该被静音。                                     |                    |
| [preload](https://www.w3school.com.cn/tags/att_audio_preload.asp) | preload  | 如果出现该属性，则音频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 |                    |
| [src](https://www.w3school.com.cn/tags/att_audio_src.asp)    | *url*    | 要播放的音频的 URL。                                         |                    |

#### JS方法

audio.play()

audio.pause()

#### JS属性

audio.muted

#### 事件

onpause

onplay

onplaying：正在播放事件

onvolumechange：音量改变