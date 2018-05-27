Penjelasan Library TimerOne :

Jika kita menggunakan board Arduino Uno, yang menjelaskan mengenai board tsersebut adalah pada bagian #elif defined(__AVR__)
sedangkan jika menggunakan mikrokontroler ATtiny85 adalah pada bagian #if defined (__AVR_ATtiny85__), dan jika kita menggunakan mikrokontroler dengan arsitektur ARM adalah pada bagian #elif defined(__arm__) && defined(CORE_TEENSY)

pada ATtiny85, Interrupt Service Routine (ISR) yang dipakai adalah IST(TIMER1_COMPA_vect) sedangkan untuk mikrokontroler jenis AVR lain menggunakan ISR(TIMER1_OVF_vect)

pada file TimerOne.h juga terdapat pilihan jika menggunakan ATtiny85 maka hanya 8 bit timer sedangkan untuk AVR yang lain maupun non AVR menggunakan timer 16 bit

Ok, sekarang kita fokus saja pada bagian AVR lain (dalam hal ini Arduino Uno) yaitu menggunakan ATmega328p. Pada Class TimerOne terdapat define dengan tulisan #elif defined (__AVR__), itulah define yang bisa digunakan jika mikrokontroler AVR yang dipakai selain ATtiny85

Terdapat suatu fungsi init timernya dengan nama initialize(), sedangkan parameter timer yang bisa dipakai dalam satuan microsecond (ms) karena memang mikrontroler menggunakan satuan tsb pada dasarnya untuk timer, pada fungsi initialize terdapat beberapa setting register yaitu TCCR1A dan TCCR1B, kenapa ada A dan B ?
karena kita menggunakan timer 16 bit, maka 8 bit ada pada register A dan 8 bit lagi ada pada register B

Kemudian, pada fungsi initialize() juga memanggil fungsi setPeriod() dengan parameter yang dimasukan adalah nilai microsecond yang nantinya digunakan untuk batas perhitungan (overflow), pada fungsi setPeriod() terdapat pula selection untuk pwmPeriod. pwmPeriode bisa diset dengan prescaler ataupun tampa prescaler, nilai prescaler ada beberapa macam diantaranya : 8, 64, 256 dan 1024

Setelah kita setting pwmPeriod tadi, maka nilai dari pwmPeriod tersebut dimasukan ke register ICR1, dan nilai dari clockSelectBits masukan pada bit register TCCR1B.

Fungsi berikutnya yang bisa digunakan adalah fungsi start() digunakan untuk start timer, juga ada fungsi resume() untuk melanjutkan, ada fungsi restart() untuk memulai ulang dan fungsi stop() untuk berhenti. Pada setiap fungsi timer tersebut terdapat register yang di set ataupun di-reset, contohnya pada fungsi start() terdapat pengosongan nilai pada TCCR1B dan TCNT1.

Pengosongan tersebut berguna untuk menentukan nilai awal timer sebelum dilalukan proses counter atau perhitungan sehingga mencapai overflow, lalu dikosongkan lagi dan dimulai lagi proses counter seperti tadi. Misalkan kita men-set interrupt pada 1 detik, maka TCNT1 akan melakukan counter sehingga mencapai nilai counter satu detik kemudian overflow

Untuk perhitungan berapa nilai overflow untuk 1 detik dapat digunakan rumus :

TCNT1 = 65536 - (delay_timer * (Xtall/Prescaler))

dengan rumus tersebut akan didapat berapa nilai overflow dari register TCNT1

Pada library TimerOne juga terdapat fungsi untuk men-set PWM, yaitu pada fungsi setPwmDuty() yang memiliki parameter pin dan duty yang diinginkan.
Dari nilai PWM yang sudah di-set tadi yaitu pwmPeriod, dimasukan ke variable dutyCycle lalu dilakukan perkalian dengan duty sehingga nanti didapat nilai dutyCycle dalam satuan persen (%)

Kemudian dilakukan operasi bitwise untuk mendapatkan nilai PWM, juga terdapat selection untuk pin yang digunakan. Jika pin tersebut adalah TIMER1_A_PIN maka nilai dutyCycle akan dimasukan ke register OCR1A.

Begitu juga dengan selection yang lain, jika pin tersebut adalah TIMER1_B_PIN, maka nilai dutyCycle akan dimasukan ke register OCR1B. Jika pin tersebut adalah TIMER1_C_PIN, maka nilai dutyCycle akan dimasukan ke register OCR1C. Namun pada Arduino Uno hanya menggunakan 2 pin saja yaitu OCR1A dan OCR1B.

Mengenai keterangan define TIMER1_A_PIN, TIMER1_B_PIN dan TIMER1_C_PIN ada pada file "known_16bit_timers.h", sedangkan jika kita menggunakan board Arduino Uno maka :

TIMER1_A_PIN = 9
TIMER1_B_PIN = 10

Sedangkan TIMER1_C_PIN tidak ada, karena memang Arduino Uno hanya memiliki 2 pin OCR1 (yaitu OCR1A dan OCR1B saja). Pada library TimerOne terdapat fungsi pwm() dengan parameter pin dan duty yang mengatur pinMode arduino sebagai output, lalu dilakukan pemanggilan fungsi setPwmDuty() untuk set.

Terdapat juga fungsi pwm() yang lain digunakan memasukan nilai delay ke fungsi setPeriod() dan juga pemanggilan fungsi pwm() untuk di-set. Fungsi ini adalah fungsi teratas yaitu yang langsung digunakan oleh programmer Arduino untuk men-set Period dan PWM.

Pada library ini juga terdapat fungsi untuk disable PWM dengan nama disablePwm(), pada fungsi ini terjadi pengosongan nilai TCCR1A sesuai dengan pin yang dipilih (sesuai paramater pin)

Kamudian terdapat juga beberapa fungsi untuk interrupt, diantaranya fungsi attachInterrupt() digunakan untuk proses routine (ISR) yang berjalan terus-menerus jika tidak di-disable

Fungsi attachInterrupt() dengan parameter microseconds adalah fungsi yang dipanggil langsung dari IDE Arduino untuk men-set interrupt, fungsi ini juga mengirimkan parameter tadi ke fungsi setPeriod untuk di-set, dan memanggil fungsi routine attachInterrupt yang akan berjalan terus-menerus jika tidak di-disable

Pada Library ini juga terdapat fungsi detachInterrupt() yang digunakan untuk menonaktifkan semua interrupt, dengan cara mengosongkan register TIMSK1, karena register TIMSK1 adalah untuk mengatur semua interrupt, timer dan counter suatu mikrokontroler

Sekian, Terima Kasih...
