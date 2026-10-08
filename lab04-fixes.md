1. Widget : StoreHeader
error / symptom: Overflow horizontal
rule broken: Constraints go down, sizes go up. 
Fix: Kolom teks dibungkus Expanded, nama maxLines: 2 + ellipsis, jam buka maxLines: 2. Rating dipindah ke baris sendiri di bawah jam buka, dengan Flexible + ellipsis.

2. widget: CategoryBar
error / symptom: Overflow horizontal.
rule broken: Sizes go up
Fix: SingleChildScrollView horizontal; tiap chip diberi jarak lewat Padding.

3. widget: PromoStrip
error / symptom: Overflow horizontal. 
rule broken: "Fix the constraint relationship, not the number".
Fix:- ListView.separated horizontal. 
    - Lebar kartu dihitung dari LayoutBuilder, bukan angka tetap.

4. widget: PromoStrip dan pemanggilnya di MenuScreen
error / symptom: Crash RangeError saat promo kurang dari 2 (termasuk zero items). Layar merah.
rule broken: Tidak ada pertahanan terhadap data tak terduga .
Fix: - PromoStrip menerima List<MenuItem>, bukan first dan second. 
     - Daftar kosong mengembalikan SizedBox.shrink(). 
     - Promo dibatasi 5 kartu.

5. widget: PromoCard
error / symptom: Overflow vertikal di dalam kartu.
rule broken: Teks panjang wajib maxLines + overflow. 
Fix: - SizedBox dihapus. 
     - Tinggi dihitung lewat PromoCard.heightFor. 
     - Spacer diganti Expanded pada nama, nama maxLines: 2, harga di FittedBox supaya tidak dipotong.

6. widget: MenuTile
error / symptom: Overflow horizontal.
rule broken: Constraints go down.
Fix: - Nama dan harga dalam Expanded, nama maxLines: 2 + ellipsis. 
     - Harga dipindah ke bawah nama, dibungkus Wrap bersama label "Promo" supaya turun baris saat sempit. 
     - Spacer dihapus.

7. widget: MenuCard (isi kartu)
error / symptom: Overflow vertikal.
rule broken: Sizes go up. 
Fix: - Nama maxLines: 2 + ellipsis, harga di FittedBox, label tombol di FittedBox. 
     - Area teks dibungkus Expanded sehingga menyerap sisa tinggi sel dan tombol selalu sejajar. 
     - Tinggi sel dihitung dari isi lewat MenuCard.heightFor.

8. widget: MenuCard (Card)
error / symptom: Overflow di bawah pada kolom nama + harga
rule broken: Ukuran yang dihitung tidak sama dengan ukuran yang benar-benar dipakai. Card punya margin bawaan 4 dp per sisi yang tidak masuk hitungan heightFor.
Fix: - margin: EdgeInsets.zero pada Card. 
     - Jarak antar sel diatur penuh oleh grid.

9. widget: MenuScreen (grid)
error / symptom: crossAxisCount: 4 tetap dan rasio sel 1:1 bawaan. Kolom tidak ikut lebar layar, dan sel terlalu kecil untuk isinya. Grid juga kebagian di layar landscape pendek.
rule broken: "Fix the constraint relationship, not the number".
Fix: SliverGridDelegateWithMaxCrossAxisExtent (maxCrossAxisExtent: 220) agar jumlah kolom diturunkan dari lebar, dengan mainAxisExtent dari MenuCard.heightFor.

10. widget: CartBar
error / symptom: Overflow horizontal
rule broken: Constraints go down, dan angka hard-code (160, 72). Parent sets position: konten tidak boleh masuk ke zona inset.
Fix: - Teks dalam Expanded, jumlah item dan total dipisah jadi dua baris. 
     - Total di FittedBox supaya tidak terpotong. 
     - Lebar tombol mengikuti isi, tinggi mengikuti isi. 
     - Material di luar, SafeArea(top: false) di dalam, jadi warna latar menutupi zona gesture bar tetapi tombol tidak.

11. widget: MenuScreen (struktur body)
error / symptom: Overflow vertikal 
rule broken: Sizes go up
Fix: Satu CustomScrollView. Header, search, chips, dan promo dalam satu SliverToBoxAdapter, diikuti sliver untuk item.

12. widget: MenuScreen (breakpoint)
error / symptom: Breakpoint memakai MediaQuery.sizeOf(context).width > 600. Large phone landscape (932) dianggap "tablet" dan mendapat grid di layar setinggi 430.
rule broken: "The breakpoint uses LayoutBuilder and CHANGES the structure at 600 dp."
Fix: LayoutBuilder membungkus body, constraints.maxWidth >= kTabletBreakpoint (600) memilih grid atau list.

13. widget: MenuScreen (list dan grid)
error / symptom: ListView(children: [...]) dan GridView.count(children: [...]) membuat objek widget untuk 500 item sekaligus. Bukan penyebab tes merah, tapi melanggar aturan lab.
rule broken: "A list you do not control is lazy."
Fix: SliverList.builder dan SliverGrid.builder. Tiap item diberi ValueKey(item.id).

14. widget: MenuScreen (empty state)
error / symptom: Tidak ada empty state sama sekali. 
rule broken: "Zero items shows an empty state: icon, message, action" dengan Key('empty-state').
Fix: Widget baru EmptyState (ikon, judul, pesan, tombol aksi) di SliverFillRemaining. Dua varian: dataset kosong ("Muat ulang") dan hasil filter kosong ("Hapus filter", memakai TextEditingController untuk mengosongkan SearchBar).

15. widget: MenuScreen (bottomNavigationBar) dan keyboard
error / symptom: Saat keyboard terbuka, CartBar tetap menempel di atas keyboard dan memakan ruang. Di landscape dengan keyboard, body hanya tersisa sekitar 12 dp.
rule broken: Sizes go up: ruang yang tersedia tidak dihormati. 
Fix: CartBar disembunyikan saat MediaQuery.viewInsetsOf(context).bottom > 0. resizeToAvoidBottomInset dibiarkan default, sehingga body menyusut dan area scroll berakhir di atas keyboard.

16. widget: MenuScreen (body) dan CartBar
error / symptom: Di landscape dengan notch, konten tertutup area kiri. Di portrait, tombol "Pesan" berada di bawah gesture bar.
rule broken: Parent sets position: konten tidak boleh berada di zona yang tidak aman.
Fix: Body dibungkus SafeArea(top: false, bottom: false) untuk inset kiri/kanan (atas ditangani AppBar, bawah oleh CartBar). CartBar memakai SafeArea(top: false).