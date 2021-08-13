# img-comparison
Сравнение картинок до/после <br>
Без использования библиотеки jQuery
Разметка:
  ```
  <div class="img-comparison">
    <img src="https://www.kartinki24.ru/uploads/gallery/main/375/kartinki24_ru_winter_156.jpg" alt="До">
    <img src="http://fullhdwallpapers.ru/image/nature/9251/les-letom.jpg" alt="После">
  </div>
  ```
Стили:
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
