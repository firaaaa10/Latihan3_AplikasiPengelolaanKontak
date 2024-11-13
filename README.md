# Aplikasi Pengelolaan Kontak

# Pembuat
Nama     : Siti Safira

NPM      : 2210010336
# 1. Deskripsi Program
Aplikasi ini memungkinkan pengguna untuk:
• Aplikasi menyimpan informasi kontak seperti nama, nomor telepon, dan kategori kontak ke dalam database SQLite.

• Kontak dapat dikelompokkan berdasarkan kategori yang dipilih dari JComboBox, seperti keluarga, teman, atau kerja.

• Mengelola kontak dengan menambah, mengedit, menghapus, dan menampilkan daftar kontak.

• Menyimpan daftar kontak ke dalam file CSV dan memuat data kontak dari file CSV.

• Mencari kontak berdasarkan nama atau nomor telepon.

• Memastikan nomor telepon yang dimasukkan hanya berupa angka dengan panjang yang sesuai.

# 2. Komponen GUI: JFrame, JPanel, JLabel, JTextField, JButton, JList, JComboBox, JTable, JScrollPane
• JFrame: Window utama aplikasi.
• JPanel: Panel untuk menampung komponen.
• JLabel: Label untuk menampilkan informasi input.
• JTextField: Input untuk nama, nomor telepon, kategori, dan pencarian.
• JButton: Tombol untuk menambah, mengedit, menghapus, mencari, mengekspor, dan mengimpor kontak.
• JComboBox: Dropdown untuk memilih kategori kontak.
• JTable: Tabel untuk menampilkan daftar kontak.

# 3. Logika Program: database SQLite, fitur CRUD (Create, Read, Update, Delete)
• Menggunakan database SQLite untuk menyimpan data kontak secara lokal.
• Menampilkan daftar kontak di JTable.
• Menyimpan dan memuat daftar kontak dari file CSV.
• Validasi nomor telepon agar hanya berupa angka dan memiliki panjang yang sesuai.

# 4. Events:
• ActionListener untuk tombol Tambah, Edit, Hapus, dan Cari Kontak.
# A. Tambah
Menambahkan kontak baru berdasarkan input dari pengguna.
```
private void btnTambahActionPerformed(java.awt.event.ActionEvent evt) {                                          
    String nama = txtNama.getText();
    String nomor = txtNomorTelepon.getText();
    String kategori = (String) comboKategori.getSelectedItem();
    
    if (!nama.isEmpty() && !nomor.isEmpty() && kategori != null) {
        dbHelper.tambahKontak(nama, nomor, kategori);
        loadKontakData(); // Refresh data di JTable
        txtNama.setText("");
        txtNomorTelepon.setText("");
        comboKategori.setSelectedIndex(0);
    } else {
        JOptionPane.showMessageDialog(this, "Kontak Berhasil Ditambahkan!");
    }

    }      
```
# B. Edit 
```
  private void btnEditActionPerformed(java.awt.event.ActionEvent evt) {                                        
    int selectedRow = tableKontak.getSelectedRow();
    
    // Pastikan ada baris yang dipilih
    if (selectedRow != -1) {
        int id = Integer.parseInt((String) tableKontak.getValueAt(selectedRow, 0)); // Ambil ID dari kolom pertama
        String nama = txtNama.getText();
        String nomor = txtNomorTelepon.getText();
        String kategori = (String) comboKategori.getSelectedItem();
        
        // Validasi data sebelum diperbarui
        if (!nama.isEmpty() && !nomor.isEmpty() && kategori != null) {
            dbHelper.updateKontak(id, nama, nomor, kategori); // Update kontak di database
            loadKontakData(); // Refresh data di JTable
            JOptionPane.showMessageDialog(this, "Kontak berhasil diperbarui!");

            // Kosongkan input setelah diperbarui
            txtNama.setText("");
            txtNomorTelepon.setText("");
            comboKategori.setSelectedIndex(0);
        }
        } else {
        JOptionPane.showMessageDialog(this, "Pilih Kontak yang Ingin Diedit!");
    
    }
    } 
```
# C. Hapus
```
private void btnHapusActionPerformed(java.awt.event.ActionEvent evt) {                                         
    int selectedRow = tableKontak.getSelectedRow();
    if (selectedRow != -1) {
        int id = Integer.parseInt((String) tableKontak.getValueAt(selectedRow, 0));
        dbHelper.hapusKontak(id);
        loadKontakData(); // Refresh data di JTable
    } else {
        JOptionPane.showMessageDialog(this, "Kontak Berhasil Dihapus!");
    }
    }   
```
# D. Cari Kontak
Mencari kontak berdasarkan nama atau nomor telepon yang dimasukkan dan menampilkan hasil di JTable.
```
private void btnCariActionPerformed(java.awt.event.ActionEvent evt) {                                        
    String keyword = txtCari.getText();
    DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
    
    dbHelper.getKontakList().forEach((kontak) -> {
        if (kontak[1].contains(keyword) || kontak[2].contains(keyword)) {
            model.addRow(kontak);
          }
        });
        tableKontak.setModel(model);
    }


    private void loadKontakData() {
        DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
        dbHelper.getKontakList().forEach((kontak) -> {
            model.addRow(kontak);
        });
        tableKontak.setModel(model);
    }        
```

• ItemListener untuk JComboBox kategori kontak

# 5. Variasi:
# a. Fitur Pencarian Kontak
Pengguna dapat mencari kontak berdasarkan nama atau nomor telepon. Hasil pencarian akan ditampilkan di JTable.
```
private void btnCariActionPerformed(java.awt.event.ActionEvent evt) {                                        
    String keyword = txtCari.getText();
    DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
    
    dbHelper.getKontakList().forEach((kontak) -> {
        if (kontak[1].contains(keyword) || kontak[2].contains(keyword)) {
            model.addRow(kontak);
          }
        });
        tableKontak.setModel(model);
    }


    private void loadKontakData() {
        DefaultTableModel model = new DefaultTableModel(new String[]{"ID", "Nama", "Nomor", "Kategori"}, 0);
        dbHelper.getKontakList().forEach((kontak) -> {
            model.addRow(kontak);
        });
        tableKontak.setModel(model);
    } 
```
# b. Validasi input 
untuk memastikan nomor telepon yang dimasukkan hanya berisi angka dan memiliki panjang yang sesuai
```
 private void txtNomorTeleponKeyTyped(java.awt.event.KeyEvent evt) {                                         
    char c = evt.getKeyChar(); 
    if (!Character.isDigit(c)) { evt.consume();
    JOptionPane.showMessageDialog(null, "Masukkan hanya angka untuk nomor telepon."); } 
    }  
```
# c. Fitur untuk mengekspor daftar kontak ke file CSV danmengimpor kontak dari file CSV ke database.
# • Aplikasi dapat mengekspor data kontak ke dalam file CSV.
```
private void eksporKeCSV() {
    try (FileWriter writer = new FileWriter("kontak.csv")) {
        // Tulis header kolom
        writer.append("ID,Nama,Nomor,Kategori\n");

        // Dapatkan daftar kontak dari database dan tulis ke CSV
        for (String[] kontak : dbHelper.getKontakList()) {
            writer.append(String.join(",", kontak));
            writer.append("\n");
        }
        
        JOptionPane.showMessageDialog(this, "Data berhasil diekspor ke kontak.csv!");
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "Gagal mengekspor data!", "Error", JOptionPane.ERROR_MESSAGE);
    }
}
```
# action listener pada button ekspor
```
private void btnEksporActionPerformed(java.awt.event.ActionEvent evt) {                                          
    btnEkspor.addActionListener(e -> eksporKeCSV());
    }

```
# • Aplikasi dapat mengimpor data kontak dari file CSV.
```
private void imporDariCSV() {
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
        loadKontakData(); // Refresh data di JTable setelah impor
    } catch (IOException e) {
        JOptionPane.showMessageDialog(this, "Gagal mengimpor data!", "Error", JOptionPane.ERROR_MESSAGE);
    }
}
```
# action listener pada button impor
```
private void btnImporActionPerformed(java.awt.event.ActionEvent evt) {                                         
     btnImpor.addActionListener(e -> eksporKeCSV());
    }  
```
# 6. Tampilan Pada Saat Aplikasi Di Jalankan
![]()

## Indikator Penilaian:

| No  | Komponen         |  Persentase  |
| :-: | --------------   |   :-----:    |
|  1  | Komponen GUI     |    20    |
|  2  | Logika Program   |    30    |
|  3  |  Events          |    15    |
|  4  | Kesesuaian UI    |    15    |
|  5  | Memenuhi Variasi |    20    |
|     | *TOTAL*        | *100* |
