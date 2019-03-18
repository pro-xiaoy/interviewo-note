## DOM
### 事件委托
~~~js
    function on(ul, methods, li, fn) {
        ul.addEventlistener(methods, function(e) {
            let el = e.target;
            while(!el.matches(li)) {
                if(el === ul) {
                    el = null
                    break;
                }
                el = el.parentNode
            }
            el && fn.call(el, e, el)
        })
        return ul
    }
~~~

### 写一个可以移动的dom
~~~html
    <style>
        #xx {
            height: 100px;
            width: 100px;
            position: absolute;
            border: 1px solid red;
        }
    </style>
    
    <DIV id='xx'></DIV>
    <script>
        var moveLoad = false;
        var position = [];
        xx.addEventListener('mousedown', function(e) {
            moveLoad = true;
            position = [e.clientX, e.clientY]
        });
        xx.addEventListener('mousemove', function(e) {
            if(moveLoad) {
                var x = e.clientX;
                var y = e.clientY;
                var moveX = x - position[0];
                var moveY = y - position[1];
                var left = parseInt(xx.style.left || 0);
                var top = parseInt(xx.style.top || 0);
                xx.style.left = moveX + left + 'px';
                xx.style.top = moveY + top + 'px';
                postion = [x, y]
            }
        });
        xx.addEventListener('mouseup', function(e) {
            moveLoad = false;
        })
    </script>
~~~