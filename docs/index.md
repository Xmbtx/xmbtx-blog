# 凶猫不太凶の虚拟机之旅

## 2023.10.17 连网修复

   - 网络报错后续

     靠，为什么上次没搜到这个解决方案

     ```
     sudo service NetworkManager stop
     ```
     
     ``` 
     sudo rm /var/lib/NetworkManager/NetworkManager.state
     ```
     
     ```
     sudo service NetworkManager start
     ```
     
     [原文链接](https://blog.csdn.net/weixin_44126988/article/details/128581200)

## 2023.10.16 更新，载入

   - 更新：

     ```
     bash auto-update-this-repo.sh
     ```

   - 妈的网络又要手动配置真的服了

     <details>
         <summary>报错图片</summary>
         <p>
             <a href="https://xmbtx.github.io/xmbtx-blog/networkW1.png"><img src="networkW1.png"/></a>
             <a href="https://xmbtx.github.io/xmbtx-blog/networkW2.png"><img src="networkW2.png"/></a>
         </p>
     </details>


     之前的解决方案：
    
     ``` 
     sudo dhclient ens33
     ```

- 图片加载：

  ![0](./picture load.png)







测试区

[![图片](networkW1.png)](javascript:void(0);)

<div id="image-modal" class="modal">
  <span class="close" onclick="closeImageModal()">&times;</span>
  <img src="networkW1.png" id="modal-image">
</div>

<script>
function openImageModal() {
  var modal = document.getElementById("image-modal");
  var modalImage = document.getElementById("modal-image");
  modal.style.display = "block";
  modalImage.src = "networkW1.png";
}
function closeImageModal() {
  var modal = document.getElementById("image-modal");
  modal.style.display = "none";
}
</script>
