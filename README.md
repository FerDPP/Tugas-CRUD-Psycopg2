import psycopg2 as db
import os

con = None
connected = None
cursor = None
def connect():
    global connected
    global con
    global cursor
    
    try:
        con = db.connect(
        host = "localhost",
        database = "sekolah",
        port = 5432,
        user = "ferdy",
        password = "567"
        )
        cursor=con.cursor()
        connected = True
    except:
        connected = False
    return cursor

def disconnect():
    global connected
    global con
    global cursor
    if(connected==True):
        cursor.close()
        con.close()
    else:
        con = None
    connected = False

def tampil():
    sql = "select * from mahasiswa"
    a = connect()
    a.execute(sql)
    record = a.fetchall()
    print(record)

def Entry():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM : ")
    xnama = input("Masukkan Nama Lengkap : ")
    xidfk = input("Masukkan ID Fakultas (1 .. 5) : ")
    xidpr = input("Masukkan ID Prodi (1 .. 10) : ")
    a = connect()
    sql = "insert into mahasiswa (nim, nama, idfakultas, idprodi) values ('"+xnim+"','"+xnama+"','"+xidfk+"','"+xidpr+"')"
    a.execute(sql)
    con.commit()
    print("Entry is done.")

def Cari():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM yang dicari : ")
    a = connect()
    sql = "select * from mahasiswa where nim ='"+xnim+"'"
    a.execute(sql)
    record = a.fetchall()
    print(record)
    print("Search is done.")

def Ubah():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM yang dicari : ")
    a = connect()
    sql = "select * from mahasiswa where nim ='"+xnim+"'"
    a.execute(sql)
    record = a.fetchall()
    print("Data saat ini : ")
    print(record)
    row = a.rowcount
    if(row==1):
        print("Silahkan untuk mengubah data..")
        xnama = input("Masukkan Nama Lengkap : ")
        xidfk = input("Masukkan ID Fakultas (1 .. 5) : ")
        xidpr = input("Masukkan ID Prodi (1 .. 10) : ")
        a = connect()
        sql = "Update mahasiswa set nama='"+xnama+"', idfakultas='"+xidfk+"', idprodi='"+xidpr+"' where nim='"+xnim+"'"
        a.execute(sql)
        con.commit()
        print("Update is done.")
        sql = "select * from mahasiswa where nim ='"+xnim+"'"
        a.execute(sql)
        record = a.fetchall()
        print("Data setelah diubah : ")
        print(record)

    else:
        print("Data tidak ditemukan")

def Hapus():
    global connected
    global con
    global cursor
    xnim = input("Masukkan NIM yang dicari : ")
    a = connect()
    sql = "select * from mahasiswa where nim ='"+xnim+"'"
    a.execute(sql)
    record = a.fetchall()
    print("Data saat ini : ")
    print(record)
    row = a.rowcount
    if(row==1):
        jwb=input("Apakah anda ingin menghapus data? (y/t)")
        if(jwb.upper()=="Y"):
            a = connect()
            sql = "delete from mahasiswa where nim ='"+xnim+"'"
            a.execute(sql)
            con.commit()
            print("Delete is done.")
        else:
            print("Data batal untuk dihapus.")
    else:
        print("Data tidak ditemukan")

Hapus()


def show_menu():
  print("=== APLIKASI DATABASE PYTHON ===")
  print("1. Insert Data")
  print("2. Tampilkan Data")
  print("3. Update Data")
  print("4. Hapus Data")
  print("5. Cari Data")
  print("0. Keluar")
  print("------------------")
  menu = input("Pilih menu> ")

  #clear screen
  os.system("cls")

  if menu == "1":
    Entry()
  elif menu == "2":
    tampil()
  elif menu == "3":
    Ubah()
  elif menu == "4":
    Hapus()
  elif menu == "5":
    Cari()
  elif menu == "0":
    exit()
  else:
    print("Menu salah!")


if __name__ == "__main__":
  while(True):
    show_menu()
