## node mock

使用node进行接口转发，转发之后获取接口数据，将数据保留在本地，下次再通过接口去请求的时候会先请求对应的mock数据

### 操作

1. express
2. http
3. fs
4. path

使用了express框架来处理代码逻辑，添加http来进行接口访问

``` javascript

app.all('/*', (req,res) => {

    const request = /* your request */;
    
    // 获取接口名称对应的文件名
    let mockStr = /your flag/.test(req.url) ? req.url.split('/').filter(item => item).join('_') : '';
    // 路径
    const filePath = path.json(__dirname, `./mock/${mockStr}`);
    if (fs.existsSync(filePath)) {
        // 存在文件就直接读取文件内容不去重复调用接口
        const mockJson = fs.readFileSync(filePath);
        res.status(200).send(JSON.parse(mockJson));
    } else {
        const r = http.request(request, (httpRes) => {
            let content = '';
            httpRes.setEncoding('utf-8');

            httpRes.on('data', (chunk) => {
                content += chunk;
            })

            httpRes.on('end', () => {
                /* 你的判断，或者逻辑 */

                if (mockStr && /* 其他逻辑 */) {
                    // 文件不存在就请求完接口，写入文件
                    fs.writeFile(filePath, JSON.stringify(content), 'utf-8', (err) => {
                        if(!err){
                            res.status(httpRes.statusCode).send(content);
                        }
                    })
                }

            })
        })
    }

    if (req.method !== 'GET' && req.body) {
        r.write(req.body);
    }
    r.end();

})

```
