一个可以通过点击左右按钮实现翻页的页面，同时加入了可以自由选择的左右翻页，渐变翻页，放大缩小翻页三种效果的页面
代码：
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Carousel</title>
    
    <style>
        .container {
           max-width: 800px;
           margin: 30px auto;
           border-radius: 4px;
           box-shadow: 0 0 4px 0 rgba(0, 0, 0, 0.3);
           padding: 16px;
        }   

        .carousel {
           position: relative;   /*相对容器绝对定位*/
           height: 200px;
           overflow: hidden;     /* 溢出隐藏，只显示 $to,隐藏掉 $from 动画*/
        }

        .carousel .panels > a {
           position: absolute; 
           display: flex;
           align-items: center;   /*因为有文字*/
           justify-content: center;
           width: 100%;
           height: 100%;    /*高度由父亲 .carousel 的 height 控制*/
           text-decoration: none;
           z-index: 1;
           
        }

        .carousel .panels > a.active {
            opacity: 1;
            z-index: 10;
    }

        .carousel .panels > a:nth-child(even) {
            background-color: lightskyblue
        }

        .carousel .panels > a:nth-child(odd) {
            background-color: lightpink
        }

        .carousel .arrow {
            position: absolute;
            top: 50%;
            z-index: 100;
            display: flex;
            align-items: center;
            justify-content: center;
            width: 32px;
            height: 32px;
            border: none;     /*把点击的按钮去掉黑色边框，变成透明色*/
            border-radius: 50%;   /*变成圆*/
            background: rgba(31,45,61,.11);
            opacity: 0;
            transition: all .3s;
            outline: none;
            cursor: pointer;
        }

        .carousel .arrow-pre {
            left: 10px;
            transform: translateX(-10px) translateY(-50%);
        }

        .carousel:hover .arrow-pre {
            transform: translateX(0) translateY(-50%);
            opacity: 1;
        }

        .carousel .arrow-next {
            right: 10px;
            transform: translateX(10px) translateY(-50%);            
        }

        .carousel:hover .arrow-next {
            transform: translateX(0) translateY(-50%);
            opacity: 1;
        }

        .carousel .arrow::before {
            content: '';
            display: block;
            width: 6px;
            height: 6px;   
            border-left: 1px solid #fff;
            border-top: 1px solid #fff;
            transform: rotate(-45deg);
        }

        .carousel .arrow.arrow-next::before {
            transform: rotate(135deg);
        }

        .carousel .indicators {
            position: absolute;
            z-index: 100;
            left: 50%;                    /*绝对定位的水平居中*/ 
            bottom: 10px; 
            transform: translateX(-50%);  /*绝对定位的居中*/
            list-style: none;    /*消除4个li的点样式*/
            margin: 0;
            padding: 0;
        }

        .carousel .indicators > li {    /* 把之前加的 li 的样式变成伪元素，然后把 li 变成父亲加pandding  扩大用户鼠标点击范围 */
            display: inline-block;
            padding: 5px 0;   /*上下5px  左右为 0 */
            cursor: pointer;
        }

        .carousel .indicators > li::before {     /*变成伪元素*/
            content: '';
            display: block;                  
            width: 30px;
            height: 2px;
            border-radius: 2px;
            background: #c0c4cc;
            transition: all .3s;
        }

        .carousel .indicators > li.active::before {
            background-color: #fff;
        }

    </style>
</head>
<body>
    <div class="container">
      <h2>Carousel</h2>
      <div>
        <select id="animation-select">    <!-- select 下拉选项标签 -->
          <option value="slide">slide</option>
          <option value="fade">fade</option>
          <option value="zoom">zoom</option>
        </select>
      </div>
      <div class="carousel">
        <div class="panels">
          <a class="active" href="#0">0</a>
          <a href="#1">1</a>
          <a href="#2">2</a>
          <a href="#3">3</a>
        </div>
        <div class="arrows">
          <button class="arrow arrow-pre"></button>
          <button class="arrow arrow-next"></button>
        </div>
        <ul class="indicators">
          <li class="active"></li>
          <li></li>
          <li></li>
          <li></li>
        </ul>
      </div>

    </div>

    <script>
      const css = ($node, cssObj) => Object.assign($node.style, cssObj)  /*把 cssObj 赋值到 $node.style 上，这样可以直接用 css 控制 */
      
      const Animation = {                       
        slide($from, $to, direction) {       /* 翻页效果 */   
          $from.style = ''
          $to.style = ''
           
            css($from, {
              transform:`translateX(0)`,
              zIndex: 10
            })
            css($to, {
              transform: `translateX(${direction === 'riaght'? '-' : ''}100%)`,
              zIndex: 10
            })           /* $ 符号后面跟{}，  ?  是紧跟在 '' 之后的 */

            setTimeout(() => {
              css($from, {
                transform:`translateX(${direction === 'left'? '-' : ''}100%)`,
                transition: `all .4s`
              })
              css($to, {
                transform: `translateX(0)`,
                transition: `all .4s`
              })
            })          
        },

        fade($from, $to) {                    /* 渐变效果 */
          $from.style = ''
          $to.style = ''
           
            css($from, {
              opacity: 1,
              zIndex: 10
            })
            css($to, {
              opacity: 0,
              zIndex: 9
            })          

            setTimeout(() => {
              css($from, {
                opacity: 0,
                zIndex: 10,
                transition: `all .4s`
              })
              css($to, {
                opacity: 1,
                zIndex: 9,
                transition: `all .4s`
              })
            })
            setTimeout(() => {
              css($from, {
                zIndex: 9
              })
              css($to, {
                zIndex: 10
              })
            },400)
        },

        zoom($from, $to) {                     /* 放大缩小效果 */
          $from.style = ''
          $to.style = ''
            
            css($from, {
              transform: `scale(1)`,
              opacity: 1,
              zIndex: 10
            })
            
            css($to, {
              transform: `scale(10)`,
              opacity: 0,
              zIndex: 9
            })          

            setTimeout(() => {
              css($from, {
                transform: `scale(10)`,
                opacity: 0,
                zIndex: 10,
                transition: `all .4s`
              })
              
              css($to, {
                transform: `scale(1)`,
                opacity: 1,
                zIndex: 9,
                transition: `all .4s`
              })
            })
           
            setTimeout(() => {
              css($from, {
                zIndex: 9
              })
             
              css($to, {
                zIndex: 10
              })
            },400)
        }
      }

      class Carousel {         
        constructor($root, animation) {
          this.$root = $root
          this.$pre = $root.querySelector('.arrow-pre')
          this.$next = $root.querySelector('.arrow-next')
          this.$$indicators = $root.querySelectorAll('.indicators > li')
          this.$$panels = $root.querySelectorAll('.panels > a')
          this.animation = animation

          this. bind()
        }

        bind() {
          this.$pre.onclick = () => {                   /*使用箭头函数就可以用this  而不用self */
            let fromIndex = this.getIndex()
            let toIndex = this.getPreIndex()
            this.setPage(fromIndex, toIndex, 'right')
            this.setIndicator(toIndex)
          }

          this.$next.onclick = () => {
          let fromIndex = this.getIndex()    /*此处 fromIndex 前面的 $ 应该去掉*/
          let toIndex = this.getNextIndex()
            this.setPage(fromIndex, toIndex, 'left')
            this.setIndicator(toIndex)
          }

          this.$$indicators.forEach($indicator => $indicator.onclick = (e) => {
            let fromIndex = this.getIndex()    
            let toIndex = [...this.$$indicators].indexOf(e.target)
            this.setIndicator(toIndex)
            let direction = fromIndex > toIndex ? 'right' : 'left'
            this.setPage(fromIndex, toIndex, direction)
          })

        }

        getIndex() {
          return [...this.$$indicators].indexOf(this.$root.querySelector('.indicators .active'))
        }

        getPreIndex() {
          return (this.getIndex() - 1 + this.$$indicators.length) % this.$$indicators.length
        }

        getNextIndex() {
          return (this.getIndex() + 1) % this.$$indicators.length
        }

        setPage(fromIndex, toIndex, direction) {
         this. animation(this.$$panels[fromIndex], this.$$panels[toIndex], direction)
        }

        setIndicator(index) {
          this.$$indicators.forEach($indicator => $indicator.classList.remove('active'))
          this.$$indicators[index].classList.add('active')
        }

        setAnimation(animation) {
          this.animation = animation
        }
      }

      let $carousel = document.querySelector('.carousel')
      let carousel = new Carousel($carousel, Animation.slide)    /*让所有组件可互不干扰运行*/
      
      document.querySelector('#animation-select').onchange =function() {
        carousel.setAnimation(Animation[this.value])   /*this.value 是 指slide,fade,zoom. 而这些又是Carousel 的属性 */
      }
      /*类数组不能直接绑定事件，需要遍历对象*/


    </script>

</body>
</html>
