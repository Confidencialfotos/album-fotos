using System;
using System.Collections.Generic;
using System.Drawing;
using System.Windows.Forms;

namespace AlbumFotos
{
    public partial class Form1 : Form
    {
        private List<Image> fotos = new List<Image>();
        private int indiceFotoAtual = 0;
        private Size tamanhoImagem = new Size(300, 200); // Tamanho padrão das imagens no álbum

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            // Carrega as imagens (substitua com suas próprias imagens)
            CarregarFotos();
            // Mostra a primeira foto
            MostrarFoto();
        }

        private void CarregarFotos()
        {
            try
            {
                // Substitua pelos caminhos das suas imagens
                fotos.Add(Image.FromFile("foto1.jpg"));
                fotos.Add(Image.FromFile("foto2.png"));
                fotos.Add(Image.FromFile("foto3.gif"));
                // Adicione mais fotos conforme necessário
            }
            catch (Exception ex)
            {
                MessageBox.Show("Erro ao carregar as imagens: " + ex.Message);
            }
        }

        private void MostrarFoto()
        {
            if (fotos.Count > 0)
            {
                // Redimensiona a imagem para o tamanho desejado
                Image imagemRedimensionada = RedimensionarImagem(fotos[indiceFotoAtual], tamanhoImagem);
                // Define a imagem no PictureBox
                pictureBox1.Image = imagemRedimensionada;
                // Atualiza o label com o número da foto
                label1.Text = $"Foto {indiceFotoAtual + 1} de {fotos.Count}";
            }
            else
            {
                pictureBox1.Image = null;
                label1.Text = "Nenhuma foto encontrada.";
            }
        }

        private Image RedimensionarImagem(Image imagemOriginal, Size tamanhoDesejado)
        {
            Bitmap bitmap = new Bitmap(tamanhoDesejado.Width, tamanhoDesejado.Height);
            using (Graphics g = Graphics.FromImage(bitmap))
            {
                g.InterpolationMode = System.Drawing.Drawing2D.InterpolationMode.HighQualityBicubic;
                g.DrawImage(imagemOriginal, 0, 0, tamanhoDesejado.Width, tamanhoDesejado.Height);
            }
            return bitmap;
        }

        private void buttonAnterior_Click(object sender, EventArgs e)
        {
            if (indiceFotoAtual > 0)
            {
                indiceFotoAtual--;
                MostrarFoto();
            }
        }

        private void buttonProximo_Click(object sender, EventArgs e)
        {
            if (indiceFotoAtual < fotos.Count - 1)
            {
                indiceFotoAtual++;
                MostrarFoto();
            }
        }
