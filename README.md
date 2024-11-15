# AplikasiPengelolaanKontak_Lat3
 Ghina Nafsi-2210010324
# 1. Deskripsi Program
Aplikasi ini memungkinkan pengguna untuk:

* Aplikasi menyimpan informasi kontak seperti nama, nomor telepon, dan kategori kontak ke dalam database SQLite.

* Kontak dapat dikelompokkan berdasarkan kategori yang dipilih dari JComboBox, seperti keluarga, teman, atau kerja.

* Mengelola kontak dengan bisa menambah, mengedit, menghapus, dan menampilkan daftar kontak.

* Menyimpan daftar kontak ke dalam file CSV dan memuat data kontak dari file CSV ke exel.

* Mencari kontak berdasarkan nama atau nomor telepon.

* Memastikan nomor telepon yang dimasukkan hanya berupa angka dengan panjang yang sesuai.

# 2. Komponen GUI: JFrame, JPanel, JLabel, JTextField, JButton, JList, JComboBox, JTable, JScrollPane
* JFrame: Window utama aplikasi.

* JPanel: Panel untuk menampung komponen-komponen.

* JLabel: Label untuk menampilkan informasi yang diberi text.

* JTextField: Input untuk nama, nomor telepon, kategori, dan pencarian.

* JButton: Tombol untuk menambah, mengedit, menghapus, mencari, mengekspor, dan mengimpor kontak.

* JComboBox: Dropdown untuk memilih kategori kontak.

* JTable: Tabel untuk menampilkan daftar kontak.

# 3. Logika Program: database SQLite, fitur CRUD (Create, Read, Update, Delete)
* Menggunakan database SQLite untuk menyimpan data kontak secara lokal.

* Menampilkan daftar kontak di JTable.

* Menyimpan dan memuat daftar kontak dari file CSV.

* Validasi nomor telepon agar hanya berupa angka dan memiliki panjang yang sesuai.

# 4. Events:
* ActionListener untuk tombol Tambah, Edit, Hapus, dan Cari Kontak.
# A. Tambah
Menambahkan kontak baru berdasarkan input dari pengguna.


private void btnInsertActionPerformed(java.awt.event.ActionEvent evt) {  

        String nama = textNama.getText();
    String nomor = textTelepon.getText();
    String kategori = (String) cmbKategori.getSelectedItem();
    
    if (!nama.isEmpty() && !nomor.isEmpty() && kategori != null) {
        dbHelper.tambahKontak(nama, nomor, kategori);
        KontakData(); // Refresh data di JTable
        textNama.setText("");
        textTelepon.setText("");
        cmbKategori.setSelectedIndex(0);
    } else {
        JOptionPane.showMessageDialog(this, "Kontak Berhasil Ditambahkan!");
    }
    }          

# B. Edit 

  private void btnUbahActionPerformed(java.awt.event.ActionEvent evt) {                                        
      
       int selectedRow = tabelKontak.getSelectedRow();
    
    // Pastikan ada baris yang dipilih
    if (selectedRow != -1) {
        int id = Integer.parseInt((String) tabelKontak.getValueAt(selectedRow, 0)); // Ambil ID dari kolom pertama
        String nama = textNama.getText();
        String nomor = textTelepon.getText();
        String kategori = (String) cmbKategori.getSelectedItem();
        
        // Validasi data sebelum diperbarui
        if (!nama.isEmpty() && !nomor.isEmpty() && kategori != null) {
            dbHelper.updateKontak(id, nama, nomor, kategori); // Update kontak di database
            KontakData(); // Refresh data di JTable
            JOptionPane.showMessageDialog(this, "Kontak berhasil Diperbarui!");

            // Kosongkan input setelah diperbarui
            textNama.setText("");
            textTelepon.setText("");
            cmbKategori.setSelectedIndex(0);
        }
        } else {
        JOptionPane.showMessageDialog(this, "Pilih Kontak yang Ingin Diedit!");
    
    }
    }   

# C. Hapus

   private void btnDeleteActionPerformed(java.awt.event.ActionEvent evt) {  
    
       int selectedRow = tabelKontak.getSelectedRow();
    if (selectedRow != -1) {
        int id = Integer.parseInt((String) tabelKontak.getValueAt(selectedRow, 0));
        dbHelper.hapusKontak(id);
        KontakData(); // Refresh data di JTable
    } else {
        JOptionPane.showMessageDialog(this, "Kontak Berhasil Dihapus!");
    }
    } 
    
# D. Cari Kontak
Mencari kontak berdasarkan nama atau nomor telepon yang dimasukkan dan menampilkan hasil di JTable.
private void btnSearchActionPerformed(java.awt.event.ActionEvent evt) {                                          
      
        String keyword = cariKontak.getText();
    DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
    
    dbHelper.getKontakList().forEach((kontak) -> {
        if (kontak[1].contains(keyword) || kontak[2].contains(keyword)) {
            model.addRow(kontak);
          }
        });
        tabelKontak.setModel(model);
    }                                         
 
    private void KontakData() {
         DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
        dbHelper.getKontakList().forEach((kontak) -> {
            model.addRow(kontak);
        });
        tabelKontak.setModel(model);
    }   


* ItemListener untuk JComboBox kategori kontak

# 5. Variasi:
# a. Fitur Pencarian Kontak
Pengguna dapat mencari kontak berdasarkan nama atau nomor telepon. Hasil pencarian akan ditampilkan di JTable.

private void btnSearchActionPerformed(java.awt.event.ActionEvent evt) {                                          
        
        String keyword = cariKontak.getText();
    DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
    
    dbHelper.getKontakList().forEach((kontak) -> {
        if (kontak[1].contains(keyword) || kontak[2].contains(keyword)) {
            model.addRow(kontak);
          }
        });
        tabelKontak.setModel(model);
    }                                         
 
    private void KontakData() {
         DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
        dbHelper.getKontakList().forEach((kontak) -> {
            model.addRow(kontak);
        });
        tabelKontak.setModel(model);
    }

# b. Validasi input 
untuk memastikan nomor telepon yang dimasukkan hanya berisi angka dan memiliki panjang yang sesuai

 private void textTeleponKeyTyped(java.awt.event.KeyEvent evt) {                                     
        
         char c = evt.getKeyChar(); 
    if (!Character.isDigit(c)) { evt.consume();
    JOptionPane.showMessageDialog(null, "Masukkan hanya angka untuk nomor telepon."); } 
    }   

# c. Fitur untuk mengekspor daftar kontak ke file CSV danmengimpor kontak dari file CSV ke database.
# • Aplikasi dapat mengekspor data kontak ke dalam file CSV.

private void EksporKeCSV() {
    
    try (FileWriter writer = new FileWriter("kontak.csv")) {
        // Tulis header kolom
        writer.append("NAMA,NOMOR,KATEGORI\n");

        // Ambil model tabel
        DefaultTableModel model = (DefaultTableModel) tabelKontak.getModel();
        int rowCount = model.getRowCount();

        // Iterasi setiap baris di tabel dan tulis ke CSV
        for (int i = 0; i < rowCount; i++) {
            String nama = (String) model.getValueAt(i, 1); // Kolom Nama
            String nomor = (String) model.getValueAt(i, 2); // Kolom Nomor
            String kategori = (String) model.getValueAt(i, 3); // Kolom Kategori

            // Tulis data ke file CSV
            writer.append(nama).append(",").append(nomor).append(",").append(kategori).append("\n");
        }

        // Baca data dari TextField
        String namaBaru = textNama.getText().trim(); // Ambil teks dari TextField Nama
        String nomorBaru = textTelepon.getText().trim(); // Ambil teks dari TextField Nomor
        String kategoriBaru = (String) jList1.getSelectedValue(); // Ambil elemen yang dipilih dari JList


        // Validasi input (opsional)
        if (namaBaru.isEmpty() || nomorBaru.isEmpty() || kategoriBaru.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Semua field harus diisi.", "Peringatan", JOptionPane.WARNING_MESSAGE);
            return; // Keluar jika ada field kosong
        }

        // Tambahkan data baru ke tabel
        model.addRow(new Object[]{rowCount + 1, namaBaru, nomorBaru, kategoriBaru});

        // Simpan data baru ke database
        dbHelper.tambahKontak(namaBaru, nomorBaru, kategoriBaru);

        JOptionPane.showMessageDialog(this, "Data berhasil diekspor ke CSV dan ditambahkan ke tabel.");
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "Gagal mengekspor data: " + e.getMessage());
        e.printStackTrace(); // Log kesalahan
    }}


# action listener pada button ekspor

private void btnEksporActionPerformed(java.awt.event.ActionEvent evt) {                                          
    btnEkspor.addActionListener(e -> eksporKeCSV());
    }


# • Aplikasi dapat mengimpor data kontak dari file CSV.

private void ImporDariCSV() {
      
       //untuk mengimpor ke CSV
        try (BufferedReader reader = new BufferedReader(new FileReader("kontak.csv"))) {
        String line;
        boolean header = true;
        
        while ((line = reader.readLine()) != null) {
            if (header) { // Lewati baris header
                header = false;
                continue;
            }
            
            String[] data = line.split(",");
            if (data.length == 4) { // Pastikan ada 4 kolom (ID, Nama, Nomor, Kategori)
                String nama = data[1];
                String nomor = data[2];
                String kategori = data[3];
                
                dbHelper.tambahKontak(nama, nomor, kategori);
            }
        }
        
        JOptionPane.showMessageDialog(this, "Data berhasil diimpor dari kontak.csv!");
        KontakData(); // Refresh data di JTable setelah impor
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "Gagal mengimpor data!", "Error", JOptionPane.ERROR_MESSAGE);
    }
    }
   

# action listener pada button impor

private void btnImpActionPerformed(java.awt.event.ActionEvent evt) {                                       
        
          btnImp.addActionListener(e -> EksporKeCSV());
   }    

# 6. Tampilan Pada Saat Aplikasi Di Jalankan
<img width="613" alt="PengelolaanKontak" src="https://github.com/user-attachments/assets/0dd5dac1-feb6-4383-8852-4476795913d4">

## Indikator Penilaian:

| No  | Komponen         |  Persentase  |
| :-: | --------------   |   :-----:    |
|  1  | Komponen GUI     |    20        |
|  2  | Logika Program   |    30        |
|  3  |  Events          |    15        |
|  4  | Kesesuaian UI    |    15        |
|  5  | Memenuhi Variasi |    20        |
|     | TOTAL            |   100        |

NAMA : GHINA NAFSI
NPM : 2210010324
KELAS 5A TI REG PAGIBJM
