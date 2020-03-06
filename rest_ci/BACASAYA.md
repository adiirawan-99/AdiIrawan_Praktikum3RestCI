Berikut ini adalah hal-hal yang perlu dipersiapkan dalam pembuatan REST API Server.
1. Install XAMPP yang bisa didapatkan pada link berikut ini https://www.apachefriends.org/download.html . Download sesuai dengan PC anda. Kemudian install juga aplikasi Postman yang dapat diunduh pada link https://www.postman.com/downloads/ .
2. CodeIgniter 3.1.11+ , yang bisa diperoleh pada link https://codeigniter.com/ . Cara menggunakannya dapat dilihat di link https://www.malasngoding.com/pengertian-dan-cara-menggunakan-codeigniter/
3. Untuk menggunakan REST API maka diperlukan Library tambahan. Salah satu library yang dapat digunakan adalah library dari Chris Kacerguis. Untuk mendapatkannya dapat diunduh pada link berikut https://github.com/ardisaurus/ci-restserver beserta CodeIgniternya.
4. Buat sebuah database, kemudian buat juga tabel dan isi didalamnya. Setelah itu ataur konfigurasi koneksi ke database pada rest_ci/application/config/database.php
5. Buat file php baru di di rest_ci/application/controller dengan nama kontak.php.
6. Isi file kontak.php dengan script seperti dibawah ini :

<?php

defined('BASEPATH') OR exit('No direct script access allowed');

require APPPATH . '/libraries/REST_Controller.php';
use Restserver\Libraries\REST_Controller;

class Kontak extends REST_Controller {

    function __construct($config = 'rest') {
        parent::__construct($config);
        $this->load->database();
    }

    function index_get() { // Fungsi ini adalah untuk memanggil data dari database yang telah dibuat.
        $id = $this->get('id'); // Mengecek apakah ada property id pada address sehingga data yang ditampilkan dapat diseleksi berdasarkan id atau ditampikan semua
        if ($id == '') {
            $kontak = $this->db->get('telepon')->result(); // Jika id tidak ada maka data ditampilkan semua
        } else {
            $this->db->where('id', $id);
            $kontak = $this->db->get('telepon')->result(); // Data ditampilkan berdasarkan id yang terdapat di property address
        }
        $this->response($kontak, 200);
    }

    function index_post() { // Fungsi ini adalah untuk mengirim data pada database yang telah dibuat.
        $data = array( // Menerima data dan membentuknya ke dalam array
                    'id'           => $this->post('id'),
                    'nama'          => $this->post('nama'),
                    'nomor'    => $this->post('nomor'));
        $insert = $this->db->insert('telepon', $data); // Memasukkan data pada database
        if ($insert) {
            $this->response($data, 200); // Jika berhasil maka data yang dimasukkan akan dimasukkan kedalam array
        } else {
            $this->response(array('status' => 'fail', 502));
        }
    }

    function index_put() { // Fungsi ini adalah untuk melakukan perubahan atau edit pada suatu data.
        $id = $this->put('id'); // Memanggil data berdasarkan idnya 
        $data = array( // Menerima data dan menyimpannya kedalam array
                    'id'       => $this->put('id'),
                    'nama'          => $this->put('nama'),
                    'nomor'    => $this->put('nomor'));
        $this->db->where('id', $id);
        $update = $this->db->update('telepon', $data); // Menyimpan perubahan data
        if ($update) {
            $this->response($data, 200);
        } else {
            $this->response(array('status' => 'fail', 502));
        }
    }

    function index_delete() { // Fungsi ini adalah untuk melakukan hapus pada suatu data .
        $id = $this->delete('id'); // Memanggil data berdasarkan arraynya
        $this->db->where('id', $id);
        $delete = $this->db->delete('telepon'); 
        if ($delete) {
            $this->response(array('status' => 'success'), 201);// Jika berhasil maka akan tampil
        } else {
            $this->response(array('status' => 'fail', 502));// Jika gagal maka akan tampil
        }
    }

}
?>
7. Selanjutnya sumber daya dari REST API tersebut dapat dimanfaatkan dengan aplikasi web, desktop, atau mobile yang menjadi client dari REST API tersebut.