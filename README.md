<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>บันทึกข้อมูล G-RDU ร้านชำ</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-100 p-5">
    <div class="max-w-md mx-auto bg-white p-8 rounded-lg shadow-lg">
        <h2 class="text-2xl font-bold mb-6 text-center text-blue-600">ลงทะเบียนร้านชำ G-RDU</h2>
        
        <form id="g-rdu-form">
            <div class="mb-4">
                <label class="block mb-1">1. ชื่อร้านชำ</label>
                <input type="text" name="ShopName" required class="w-full border p-2 rounded">
            </div>
            <div class="mb-4">
                <label class="block mb-1">2. ชื่อเจ้าของร้าน</label>
                <input type="text" name="OwnerName" required class="w-full border p-2 rounded">
            </div>
            <div class="mb-4">
                <label class="block mb-1">3. เบอร์โทรศัพท์</label>
                <input type="tel" name="Phone" required class="w-full border p-2 rounded">
            </div>
            
            <div class="mb-4 flex gap-2">
                <div class="w-1/2">
                    <label class="block mb-1 text-sm">4. ละติจูด</label>
                    <input type="text" id="lat" name="Lat" readonly class="w-full border p-2 bg-gray-50 rounded">
                </div>
                <div class="w-1/2">
                    <label class="block mb-1 text-sm">5. ลองติจูด</label>
                    <input type="text" id="long" name="Long" readonly class="w-full border p-2 bg-gray-50 rounded">
                </div>
            </div>
            <button type="button" onclick="getLocation()" class="mb-4 w-full bg-green-500 text-white py-1 rounded hover:bg-green-600">ดึงพิกัดปัจจุบัน</button>

            <div class="mb-4">
                <label class="block mb-1">6. ผลการประเมิน G-RDU/G-SHP</label>
                <select name="Result" class="w-full border p-2 rounded">
                    <option value="ผ่านเกณฑ์">ผ่านเกณฑ์</option>
                    <option value="ไม่ผ่านเกณฑ์">ไม่ผ่านเกณฑ์</option>
                    <option value="รอปรับปรุง">รอปรับปรุง</option>
                </select>
            </div>
            <div class="mb-6">
                <label class="block mb-1">7. เขต รพ.สต.</label>
                <input type="text" name="HealthCenter" placeholder="ระบุชื่อ รพ.สต." required class="w-full border p-2 rounded">
            </div>

            <button type="submit" id="submitBtn" class="w-full bg-blue-600 text-white py-3 rounded-lg font-bold hover:bg-blue-700">บันทึกข้อมูล</button>
        </form>
    </div>

    <script>
        const scriptURL = 'https://script.google.com/macros/s/AKfycbycuC0qWDrWpNzspcCEx58l-ql7hYwCU6lqlJrBk2X6RSfYYWj3Y1nrec1SVc5GuRsr/exec';
        const form = document.getElementById('g-rdu-form');

        function getLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(pos => {
                    document.getElementById('lat').value = pos.coords.latitude;
                    document.getElementById('long').value = pos.coords.longitude;
                }, err => alert("กรุณาอนุญาตการเข้าถึงพิกัด"));
            }
        }

        form.addEventListener('submit', e => {
            e.preventDefault();
            const btn = document.getElementById('submitBtn');
            btn.disabled = true;
            btn.innerText = "กำลังบันทึก...";

            fetch(scriptURL, { method: 'POST', body: new FormData(form)})
                .then(response => {
                    alert("บันทึกข้อมูลสำเร็จ!");
                    form.reset();
                    btn.disabled = false;
                    btn.innerText = "บันทึกข้อมูล";
                })
                .catch(error => {
                    alert("เกิดข้อผิดพลาด!");
                    btn.disabled = false;
                });
        });
    </script>
</body>
</html>
