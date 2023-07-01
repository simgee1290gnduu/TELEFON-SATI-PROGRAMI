# TELEFON-SATIŞ-PROGRAMI
satış dükkanında bulunan telefonların satış ve müşteri  bilgilerini kayıt altına alınması ve takibinin yapılması amacıyla hazırlanmış access veri tabanını kullanan bir program.
telefonların ve satışı yapılan müşterilerin bilgileri access veri tabanında yapılmaktadır. Veri tabanı üzerinde kayıt ekleme, kayıt silme, kayıt listeleme, kayıt arama ve kayıt güncelleme işlemleri yapılmaktadır.
Öncelikle veri tabanında bulunan tablonun alanlarını ve veri türlerini yazıyorum.

Programla neler yapılabilir:

Yeni kayıt ekleme: Yeni bir müşteri geldiğinde müşteri bilgilerini yazarak tabloya yeni bir kayıt ekleyebiliriz.
Kayıt listeleme: Tabloda bulunan tüm kayıtları ekranda bir datagrid üzerinde listeleyebiliriz.
Kayıt Arama: İstediğimiz herhangi bir kritere göre kayıt arama yaptırabiliriz. Aranan kayıt bulunduğunda tüm bilgileri ekrana getirilir.
Kayıt Güncelleme: Tabloda bulunan herhangi bir kaydı bularak kayıt üzerinde istediğimiz gibi güncelleme işlemi yapabiliriz.
Kayıt Silme: Veri tabanından istenilen bir kayıt bulunarak veri tabanından silme işlemi yapılabilir.

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.Data.OleDb;//Veritabanı bağlantı kütüphanesi
namespace telefon
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        //Veri Tabanı Değişkenlerini Tanımlama Bölümü
        OleDbConnection baglanti = new OleDbConnection("Provider=Microsoft.ACE.OLEDB.12.0;Data Source=telefon.accdb");
        OleDbCommand komut = new OleDbCommand();
        OleDbDataAdapter adaptor = new OleDbDataAdapter();
        DataSet tasima = new DataSet();
        string resim_t;
        //DataGridWiev de kayıtları listeleme Bölümü
        void listele()
        {
            baglanti.Open();
            OleDbDataAdapter adaptor = new OleDbDataAdapter("Select * from telefon", baglanti);
            adaptor.Fill(tasima, "telefon");
            dataGridView1.DataSource = tasima.Tables["telefon"];
            adaptor.Dispose();
            baglanti.Close();
        }
        private void Form1_Load(object sender, EventArgs e)
        {
            listele();
        }
        //Resim Ekleme Butonu
        private void button7_Click(object sender, EventArgs e)
        {
            if (openFileDialog1.ShowDialog() == DialogResult.OK) 
            {
                pictureBox1.ImageLocation = openFileDialog1.FileName;
                t_resim.Text = openFileDialog1.FileName;
            }
            //"\" Karakterlerin ascii kodunu alma bölümü
            int s = 92;
            string harf = ((char)s).ToString();
            //Resmin adresinin tersten yazdırma bölümü
            string yazi = t_resim.Text; string metin = "";
            int yaziuzunlugu = yazi.Length;
            for (int i = yaziuzunlugu; i > 0; i--)
            {
                if (yazi.Substring(i - 1, 1) == harf)
                {
                    break;
                }
                metin = metin + (yazi.Substring(i - 1, 1));
            }
            // Bulunan resim adını düzden yazdırma bölümü
            int uzunluk = metin.Length; string kelime = "";
            for (int a = uzunluk; a > 0; a--)
            {
                kelime = kelime + (metin.Substring(a - 1, 1));
            }
            //resim adını oresim kutusuna yazdırma bölümü
            t_resim.Text = "Resimler/" + kelime;
            resim_t = t_resim.Text;
            
        }
        //Kayıt Ekleme Butonu
        private void button5_Click(object sender, EventArgs e)
        {
            t_resim.Text = pictureBox1.ImageLocation;
         if (t_markasi.Text != "" && t_modeli.Text != "" && t_ime_no.Text != "" && t_seri_no.Text != "" && t_islemci.Text != "" && t_hafiza.Text != "" && t_isletim_sistemi.Text != "" && t_uretim_tarihi.Text != "" && t_cozunurluk.Text != "" && t_ekran_boyutu.Text != "" && t_fiyati.Text != "" && t_resim.Text != "")
        {
           komut.Connection = baglanti;
           komut.CommandText = "Insert Into telefon(t_markasi,t_modeli,t_ime_no,t_seri_no,t_islemci,t_hafiza,t_isletim_sistemi,t_uretim_tarihi,t_cozunurluk,t_ekran_boyutu,t_fiyati,t_resim) Values ('" + t_markasi.Text + "','" + t_modeli.Text + "','" + t_ime_no.Text + "','" + t_seri_no.Text + "','" + t_islemci.Text + "','" + t_hafiza.Text + "','" + t_isletim_sistemi.Text + "','" + t_uretim_tarihi.Text + "','" + t_cozunurluk.Text + "','" + t_ekran_boyutu.Text + "','" + t_fiyati.Text + "','" + t_resim+ "')";
           baglanti.Open();
           komut.ExecuteNonQuery();
           komut.Dispose();
           baglanti.Close();
           MessageBox.Show("Kayıt Tamamlandı!");
           tasima.Clear();
           listele();
        }
           else
        {
         MessageBox.Show("Boş alan geçmeyiniz!");
           }
        }
        //Yeni Kayıt Ekleme Butonu
        private void button4_Click(object sender, EventArgs e)
        {
            t_id.Text = "";
            t_markasi.Text = "";
            t_modeli.Text = "";
            t_ime_no.Text = "";
            t_seri_no.Text = "";
            t_islemci.Text = "";
            t_hafiza.Text = "";
            t_isletim_sistemi.Text = "";
            t_uretim_tarihi.Text = "";
            t_cozunurluk.Text = "";
            t_ekran_boyutu.Text = "";
            t_fiyati.Text = "";
            t_resim.Text = "";
            pictureBox1.ImageLocation = "";
        }
        //Kayıt Silme Bölümü
        private void button1_Click(object sender, EventArgs e)
        {
            DialogResult c;
            c = MessageBox.Show("Silmek istediğinizden emin misiniz?", "Uyarı!", MessageBoxButtons.YesNo, MessageBoxIcon.Warning);
            if (c == DialogResult.Yes)
            {
                baglanti.Open();
                komut.Connection = baglanti;
                komut.CommandText = "Delete from telefon where t_id=" + textBox1.Text + "";
                komut.ExecuteNonQuery();
                komut.Dispose();
                baglanti.Close();
                tasima.Clear();
                listele();
            }
        }
        //DataGridView üzerinde tıklanan kaydın ekranda gösterilmesi
        private void dataGridView1_CellContentClick(object sender, DataGridViewCellEventArgs e)
        {
            t_id.Text = dataGridView1.CurrentRow.Cells[0].Value.ToString();
            t_markasi.Text = dataGridView1.CurrentRow.Cells[1].Value.ToString();
            t_modeli.Text = dataGridView1.CurrentRow.Cells[2].Value.ToString();
            t_ime_no.Text = dataGridView1.CurrentRow.Cells[3].Value.ToString();
            t_seri_no.Text = dataGridView1.CurrentRow.Cells[4].Value.ToString();
            t_islemci.Text = dataGridView1.CurrentRow.Cells[5].Value.ToString();
            t_hafiza.Text = dataGridView1.CurrentRow.Cells[6].Value.ToString();
            t_isletim_sistemi.Text = dataGridView1.CurrentRow.Cells[7].Value.ToString();
            t_uretim_tarihi.Text = dataGridView1.CurrentRow.Cells[8].Value.ToString();
            t_cozunurluk.Text = dataGridView1.CurrentRow.Cells[9].Value.ToString();
            t_ekran_boyutu.Text = dataGridView1.CurrentRow.Cells[10].Value.ToString();
            t_fiyati.Text = dataGridView1.CurrentRow.Cells[11].Value.ToString();
            t_resim.Text = dataGridView1.CurrentRow.Cells[12].Value.ToString();
            pictureBox1.ImageLocation = dataGridView1.CurrentRow.Cells[12].Value.ToString();
            
        }
        //Kayıt Arama Bölümü
        private void button2_Click(object sender, EventArgs e)
        {
            baglanti = new OleDbConnection("Provider=Microsoft.ACE.Oledb.12.0;Data Source=telefon.accdb");
            adaptor = new OleDbDataAdapter("SElect * from telefon where t_markasi like '%" + textBox2.Text + "%'", baglanti);
            tasima = new DataSet();
            baglanti.Open();
            adaptor.Fill(tasima, "telefon");
            dataGridView1.DataSource = tasima.Tables["telefon"];
            baglanti.Close();
            
            //Bulunan kayıt textboxlara atanarak gösteriliyor.
            t_markasi.Text = dataGridView1.CurrentRow.Cells[1].Value.ToString();
            t_modeli.Text = dataGridView1.CurrentRow.Cells[2].Value.ToString();
            t_ime_no.Text = dataGridView1.CurrentRow.Cells[3].Value.ToString();
            t_seri_no.Text = dataGridView1.CurrentRow.Cells[4].Value.ToString();
            t_islemci.Text = dataGridView1.CurrentRow.Cells[5].Value.ToString();
            t_hafiza.Text = dataGridView1.CurrentRow.Cells[6].Value.ToString();
            t_isletim_sistemi.Text = dataGridView1.CurrentRow.Cells[7].Value.ToString();
            t_uretim_tarihi.Text = dataGridView1.CurrentRow.Cells[8].Value.ToString();
            t_cozunurluk.Text = dataGridView1.CurrentRow.Cells[9].Value.ToString();
            t_ekran_boyutu.Text = dataGridView1.CurrentRow.Cells[10].Value.ToString();
            t_fiyati.Text = dataGridView1.CurrentRow.Cells[11].Value.ToString();
            t_resim.Text = dataGridView1.CurrentRow.Cells[12].Value.ToString();
        }
        //Telefon ID'ına göre arama
        private void button3_Click(object sender, EventArgs e)
        {
            OleDbConnection con = new OleDbConnection("Provider=Microsoft.ACE.Oledb.12.0;Data Source=telefon.accdb"); con.Open();
            OleDbCommand cmd = new OleDbCommand("Select * from telefon where t_id=" + textBox3.Text + "", con);
            OleDbDataReader dr = cmd.ExecuteReader();
            while (dr.Read())
            {
                t_id.Text = dr["t_id"].ToString();
                t_markasi.Text = dr["t_markasi"].ToString();
                t_modeli.Text = dr["t_modeli"].ToString();
                t_ime_no.Text = dr["t_ime_no"].ToString();
                t_seri_no.Text = dr["t_seri_no"].ToString();
                t_islemci.Text = dr["t_islemci"].ToString();
                t_hafiza.Text = dr["t_hafiza"].ToString();
                t_isletim_sistemi.Text = dr["t_isletim_sistemi"].ToString();
                t_uretim_tarihi.Text = dr["t_uretim_tarihi"].ToString();
                t_cozunurluk.Text = dr["t_cozunurluk"].ToString();
                t_ekran_boyutu.Text = dr["t_ekran_boyutu"].ToString();
                t_fiyati.Text = dr["t_fiyati"].ToString();
                pictureBox1.ImageLocation = dr["t_resim"].ToString();
                t_resim.Text = dr["t_resim"].ToString();
            }
            con.Close();
        }
        // Kayıt güncelleme bölümü
        private void button6_Click(object sender, EventArgs e)
        {
            komut = new OleDbCommand();
            baglanti.Open();
            komut.Connection = baglanti;
            komut.CommandText = "update telefon set t_markasi='" + t_markasi.Text + "', t_modeli='" + t_modeli.Text +"',t_ime_no='"+ t_ime_no.Text +"',t_seri_no='"+ t_seri_no.Text +"',t_islemci='"+ t_islemci.Text +"',t_hafiza='"+ t_hafiza.Text +"',t_isletim_sistemi='"+ t_isletim_sistemi.Text +"',t_uretim_tarihi='"+ t_uretim_tarihi.Text +"',t_cozunurluk='"+ t_cozunurluk.Text +"',t_ekran_boyutu='"+ t_ekran_boyutu.Text +"',t_fiyati='"+ t_fiyati.Text +"',t_resim='"+ t_resim.Text +"' where t_id=" + t_id.Text + "";
            komut.ExecuteNonQuery();
            baglanti.Close();
            tasima.Clear();
            listele();
        }
    }
}
