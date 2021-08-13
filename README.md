# img-comparison
Сравнение картинок до/после <br>
Без использования библиотеки jQuery<br>
Разметка:<br>
  ```
  <div class="img-comparison">
    <img src="https://www.kartinki24.ru/uploads/gallery/main/375/kartinki24_ru_winter_156.jpg" alt="До">
    <img src="http://fullhdwallpapers.ru/image/nature/9251/les-letom.jpg" alt="После">
  </div>
  ```
Стили:<br>
  ```
  .img-comparison {
  width: 100%;
  position: relative;
  overflow: hidden;
}
.img-comparison__before {
  position: absolute;
  height: 100%;
  width: 50%;
  top: 0;
  left: 0;
  overflow: hidden;
  z-index: 2;
}
.img-comparison__before img {
  height: 100%;
}
.img-comparison__after {
  height: 100%;
}
.img-comparison__after img {
  width: 100%;
  height: 100%;
  display: block;
}

.img-comparison__resizer {
  position: absolute;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 5;
  top: 0;
  left: 50%;
  height: 100%;
  width: 2px;
  background: white;
  touch-action: pan-y;
  cursor: pointer;

}
.img-comparison__resizer::after,
.img-comparison__resizer::before {
  content: "";
  position: absolute;
  top: 50%;
  width: 20px;
  height: 20px;
  border: solid white;
  border-width: 0 2px 2px 0;
  z-index: 300;
}
.img-comparison__resizer::before {
  transform: rotate(135deg);
  left: -30px;
}
.img-comparison__resizer::after {
  transform: rotate(-45deg);
  right: -30px;
}
  ```
  
JS:<br>
  ```
class Сomparison {
  constructor({parent}) {
      this.parent = document.querySelectorAll(parent); // Родительский єлемент для фотографий.
  }
  renderEl() {
    this.parent.forEach((item) => {
      const images = item.querySelectorAll("img");
      const fragment = document.createDocumentFragment();
      const beforeEl = document.createElement("div");
      const afterEl = document.createElement("div");
      const resizerEl = document.createElement("div");
      beforeEl.classList.add("img-comparison__before");
      beforeEl.appendChild(images[0]);
      afterEl.classList.add("img-comparison__after");
      afterEl.appendChild(images[1]);
      resizerEl.classList.add("img-comparison__resizer");
      fragment.appendChild(beforeEl);
      fragment.appendChild(afterEl);
      fragment.appendChild(resizerEl);
      item.appendChild(fragment);
    });
  }
 
  change() {
    this.parent.forEach((item) => {
      const before = item.querySelector(".img-comparison__before");
      const beforeImage = item.firstElementChild.querySelector('img');
      const resizer = item.querySelector(".img-comparison__resizer");
      let active = false;
      document.addEventListener("DOMContentLoaded", function () {
        let width = item.offsetWidth;
        beforeImage.style.width = width + "px";
      });
      resizer.addEventListener("mousedown", function () {
        active = true;
        resizer.classList.add("resize");
      });

      document.body.addEventListener("mouseup", function () {
        active = false;
        resizer.classList.remove("resize");
      });

      document.body.addEventListener("mouseleave", function () {
        active = false;
        resizer.classList.remove("resize");
      });
      document.body.addEventListener("mousemove", function (e) {
        if (!active) return;
        let x = e.pageX;
        x -= item.getBoundingClientRect().left;
        resizeIt(x);
        pauseEvent(e);
      });
      resizer.addEventListener("touchstart", function () {
        active = true;
        resizer.classList.add("resize");
      });
      document.body.addEventListener("touchend", function () {
        active = false;
        resizer.classList.remove("resize");
      });

      document.body.addEventListener("touchcancel", function () {
        active = false;
        resizer.classList.remove("resize");
      });
      document.body.addEventListener("touchmove", function (e) {
        if (!active) return;
        let x;

        let i;
        for (i = 0; i < e.changedTouches.length; i++) {
          x = e.changedTouches[i].pageX;
        }

        x -= item.getBoundingClientRect().left;
        resizeIt(x);
        pauseEvent(e);
      });
      function resizeIt(x) {
        let transform = Math.max(0, Math.min(x, item.offsetWidth));
        before.style.width = transform + "px";
        resizer.style.left = transform - 0 + "px";
      }
      function pauseEvent(e) {
        if (e.stopPropagation) e.stopPropagation();
        if (e.preventDefault) e.preventDefault();
        e.cancelBubble = true;
        e.returnValue = false;
        return false;
      }
    });
  }
  init() {
    this.renderEl();
    this.change();
  }
}

const beforeAfter = new Сomparison ({
  parent: '.img-comparison '
});
beforeAfter.init();
  ```


