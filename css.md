1. 简单介绍下BFC?

  BFC 块级格式化上下文, 可以简单理解为一个独立的容器, 并且这个容器的里box的布局, 与这个容器外的毫不相干。

  - 怎样才能形成BFC?

    1. float 值不为none
    2. overflow 值不为 visible
    3. display的值为 table-cell, table-caption, inline-block 中的任何一个。
    4. position 的值不为 relative 和 static。

  - 什么时候需要用到BFC?

   1. 垂直外边距重叠问题:

      1> 父子元素 margin 重叠问题:  
        ```html
          <style>
              * {
                padding: 0;
                margin: 0;
              }
              .father{
                background: pink;
                height: 100px;
                width: 400px;
              }
              .son {
                margin-top: 50px;
                background: #000;
                height: 50px;
              }
          </style>
            
          <body>
            <div class="father">
              <div class="son"></div>
            </div>
          </body>
        ``` 

      解决方案: 给父元素增加 overflow: hidden

      2> 相邻元素垂直外边距折叠问题:

      ```html
        <style>
            .gege {
              background: red;
              width: 100px;
              height: 100px;
              margin: 40px;
            }
            .didi {
              background: green;
              width: 100px;
              height: 100px;
              margin: 40px;
            }
        </style>
        <body>
          <div class="gege">gege</div>
          <div class="didi">didi</div>
        </body>
      ```
      解决办法：给元素didi外面包一个元素wrap，并触发wrap的BFC。

  2. 场景2: 浮动元素造成父元素高度坍塌问题。
     解决办法: 给父元素新增 overflow: hidden;

  3. 页面适配方案: 百分比,flex,grid,vh,vw。 

  4. (1)实现两边固定，中间自适应布局：

      flex:1

      ```html
        <html lang="en">
        <head>
          <style>
            div {
              border: 1px solid red;
              height: 200px;
            } 
            .container{
              display: flex;
            }
            .first{
              width: 200px;
            }
            .center{
              flex:1;
            }
            .last{
              width: 200px; 
            }
          </style>
        </head>
        <body>
          <div class="container">
            <div class="first">
              first
            </div>
            <div class="center">
              center
            </div>
            <div class="last">
              last
            </div>
          </div>
        </body>
        </html>
      ```

    (2) 在1的基础上调整center 的位置到中间

    ```html 
      <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
          div {
            border: 1px solid red;
            height: 200px;
          } 
          .container{
            display: flex;
          }
          .first{
            order: 2;
            width: 200px;
          }
          .center{
            order:1;
            flex:1;
          }
          .last{
            order: 3;
            width: 200px; 
          }
        </style>
      </head>
      <body>
        <div class="container">
          <div class="first">
            first
          </div>
          <div class="center">
            center
          </div>
          <div class="last">
            last
          </div>
        </div>
      </body>
      </html>
    ```
  5. flex1 是哪些属性的集合

    flex-grow:   定义项目的放大比例;

                 如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

    flex-shrink: 定义了项目的缩小比例;

                 如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。

    flex-basis:  定义项目占据的主轴空间;

                 浏览器根据这个属性，计算主轴是否有多余空间。

6. 父级垂直且水平居中的写法?
   1. 
   ```html
    <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Document</title>
          <style>
            html,body{
              width:100%;
              height:100%;
            }
            .parent{
              position:relative;
              width:100%;
              height:100%;
              background:red;
            }
            .child{
              position:absolute;
              width:200px;
              height:200px;
              top:0;
              right:0;
              bottom:0;
              left:0;
              margin: auto;
              background:green;
            }
          </style>
        </head>
        <body>
          <div class="parent"> 
            <div class="child"></div>
          </div>
        </body>
        </html>
   ```
   2. 
   ```html
      <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Document</title>
          <style>
            html,body{
              width:100%;
              height:100%;
            }
            .parent{
              width:100%;
              height:100%;
              background:red;
              position:relative;
            }
            .child{
              width: 200px;
              height: 200px;
              position: absolute;
              left: 50%;
              top: 50%;
              transform: translate(-100px,-100px);
              background: #000;
            };
          </style>
        </head>
        <body>
          <div class="parent"> 
            <div class="child"></div>
          </div>
        </body>
        </html>
    ```
    3. 
   ```html
      <!DOCTYPE html>
        <html lang="en">
        <head>
          <meta charset="UTF-8">
          <meta name="viewport" content="width=device-width, initial-scale=1.0">
          <title>Document</title>
          <style>
            html,body{
              width:100%;
              height:100%;
            }
            .parent{
              width:100%;
              height:100%;
              background:red;
              position:relative;
            }
            .child{
              width: 200px;
              height: 200px;
              position: absolute;
              left:50%;
              top:50%;  
              margin-top: -100px;
              margin-left: -100px; 
              background: #000;
            };
          </style>
        </head>
        <body>
          <div class="parent"> 
            <div class="child"></div>
          </div>
        </body>
        </html>
    ```
    4. 
    ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
          html,body{
            width:100%;
            height:100%;
          }
          .parent{
            width:100%;
            height:100%;
            background: red;
            display: flex;
            justify-content: center;
            align-items: center;
          }
          .child{
            width: 200px;
            height: 200px;
            background: #000;
          };
        </style>
      </head>
      <body>
        <div class="parent"> 
          <div class="child"></div>
        </div>
      </body>
      </html>
    ```
    

   


