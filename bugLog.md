# bug记录

### 上传文件【连续上传相同的照片没反应】
> input type=file `onChange`事件触发上传逻辑，火狐浏览器连续相同的文件是可以触发onChange事件，而`chrome浏览器` 和 `android手机`触发不了onChange事件
> 
```js
export const ImagePicker = (props: ImagePickerProp) => {
  const { files, max = 0, onImageAdd, onDel } = props
  const [key, setKey] = useState<number>(0)

  const RenderChoseBtn = () => {
    if (files.length >= max) {
      return null
    }
    return (
      <div className={styles.choseContainer}>
        <input
          key={key}      //指定input框 key
          multiple={false}
          type="file"
          accept="image/*"
          onChange={async e => {
            const { url, err } = await uploadImageFile(e)
            // 上传成功后改变input框的key值，重置了input的状态，这样连续相同的照片即可触发onChange事件
            setKey(va => va + 1)   
            if (err) {
              Toast.fail(err.toString())
            } else {
              onImageAdd(url)
            }
          }}
        />
        <img className={styles.cameraIcon} src={cameraIcon} />
        <div className={styles.txtDes}>添加照片</div>
        <div className={styles.selectedNum}>
          ({files.length}/{max})
        </div>
      </div>
    )
  }
```
