<template>
    <div class="comment">
        <div class="banner-bg" id="top">
            <div class="banner-overlay"></div>
            <div class="welcome-text">
                <h2>PhetPreng | "Shoulder Bag PreOrders"</h2>
                <h5>ร้านเพชรเพร็ง เปิดให้สั่งซื้อสั่งจองกระเป๋าสะพายข้างผ้ายีน ราคาใบละ 199 บาท มีให้เลือก 2สี 2ขนาด ตามรูป
                    <hr>
                    <center>
                   <img src="@/assets/1.jpg">
                   <img src="@/assets/2.jpg">
                   <img src="@/assets/3.jpg">
                   <img src="@/assets/4.jpg">
                   <img src="@/assets/5.jpg">
                    </center>
                    <br>
                    <h4>รายละเอียดสินค้า</h4>
                    - รูปแบบ: ธรรมดา
                    <br>
                    - สี: ผ้าสีน้ำเงินเข้ม, ผ้าสีฟ้าอ่อน
                    <br>
                    - วัสดุ: ผ้ายีนส์
                    <br>
                    - ขนาด: 39x40 และ 35x45 
                    <br>
                    - วิธีการเปิด: ซิป
                    <br>
                    - สไตล์: เกาหลี
                    <br>
                    <br>
                    <br>
                    <h4>ช่องทางการชำระเงิน</h4>
                    - พร้อมเพย์ : 0830521809
                    <br>
                    - ธนาคารกสิกร : 0568467167
                    <br>
                    - ธนาคารกรุงไทย : 8570660766
                    </h5>
            </div>
        </div>
    </div>
</template>
<script>
export default {
}
</script>
<style scoped>
    .comment {
        margin-left: 10px;
        margin-top: 10px;
        margin-right: 10px;
    }
    .banner-bg {
	    padding: 190px 80px;
	    position: relative;
	    background-position: center;
	    background-size: cover;
	    z-index: 2;
    }
    .banner-overlay {
        position: absolute;
        width: 100%;
        height: 125vh;
        top: 0;
        right: 0;
        z-index: 3;
    }
    
    img {
        width: 75%;
        height: 75%;
    }
</style>