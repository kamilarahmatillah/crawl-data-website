# Crawl Data Web

Nama : Nisaa Kamila Rahmatillah

NRP : 160411100003

Mata kuliah : Penambangan dan Pencarian Web 

Jurusan : Teknik Informatika

Dosen pengampu : Mulaab, S.Si., M.Kom.



**Langkah-langkah crawl data web:**

- install BeautifulSoup4 pada CMD mengunakan pip

- install request pada CMD menggunakan pip

- import SQLite3 

- website: 'http://www.bukabuku.com/'

- program pertama digunakan untuk mendownload kode html website yang kemudian akan  dirubah kedalam objek BeautifulSoup4

  ```python
  	page = requests.get(src)
      soup = BeautifulSoup(page.content, 'html.parser')
  ```



- setelah itu program dibawah ini berfungsi untuk mencari bagian yang akan diambil 

  ```python
      content = soup.find(class_ ='main_section_home')
      producs = content.findAll(class_ ='product_list_grid_home')
  
  ```

  pada web ini class yang akan dicari  adalah `'main_section_home'`, dan yang akan diambil adalah class `'product_list_grid_home'`. 



- selanjutnya program dibawah ini berfungsi untuk mengambil data kemudian dimasukkan ke database

  ```python
  for buku in producs:
                  #print(buku.find(class_='products_list_title'))
                  judul = list(buku.find(class_='product_list_title'))[1].getText()
                  #print(judul)
                      # Cek apakah buku tsb memiliki keterangan (penulis, kategori) atau tidak, 
  
                  tampung = buku.findAll(class_='text_smaller')
                  cover = tampung[0].getText()
                  cover = cover.split('\r')[1].split('\n')[1].split('\t')[4]
                  pengarang = tampung[1].getText()
                  pengarang = pengarang.split('\r')[1].split('\n')[1].split('\t')[4]
  
                  harga = buku.find(class_ ='price')
                  if harga == None:
                          harga = "0"
  
                  else:
                          harga = buku.find(class_ ='price').getText()
  
                  # Insert hasil crawl ke dalam database SQLite
                  conn.execute('INSERT INTO BUKABUKU \
                              VALUES ("%s", "%s", "%s", "%s")' %(judul, cover, pengarang, harga));
  ```

  

- sebelum itu membuat tabel dan isi tabel pada database yang sudah dibuat

  ```python
  conn = sqlite3.connect('nisaa.db')
  conn.execute('''CREATE TABLE if not exists BUKABUKU
          (JUDUL          TEXT     NOT NULL,
          COVER           TEXT     NOT NULL,
          PENGARANG       TEXT     NOT NULL,
          HARGA           TEXT     NOT NULL);''')
  ```

  
