1. Bagian "constraints go down" yang dilanggar, oleh Row sebagai parent. Row meneruskan lebar tak terbatas ke child non-flex, jadi Text mengukur dirinya sendiri sesuai panjang isinya dan ukuran yang ia laporkan ke atas melebihi ruang yang dimiliki Row. Perbaikan yang benar adalah membungkus Text dengan Expanded atau Flexible, supaya Row menyampaikan batas lebar ke anaknya.

2. karena menambah angka hardcoded hanya memperbaiki 1 masalah, jika kasus kayak layar lebih sempit maka overflow muncul lagi

3. Tes 7 gagal, karena shrinkWrap: true memaksa seluruh item dibangun dan diukur sekaligus sehingga MenuTile yang terbangun jauh melebihi batas 100. Begitu data datang dari API dengan ukuran yang tidak kita kontrol, list besar akan membangun semua barisnya sekaligus, sehingga membuat device tidak efisien.

4. MediaQuery.sizeOf hanya memberi ukuran seluruh jendela, bukan ruang yang benar-benar diberikan parent kepada widget itu. LayoutBuilder membaca constraint dari parent, sehingga layout beradaptasi dengan ruang yang tersedia (misalnya di panel samping atau split-screen), bukan dengan ukuran layar yang mungkin tidak relevan.

5. karena crash tersebut berasal dari bug layout yang sama dengan yg lain. kode menganggap data selalu berbentuk tertentu (promos[0] dan promos[1] selalu ada), padahal layar harus tahan terhadap data apa pun yang diterima (nol item, nama 200 karakter, 500 item).