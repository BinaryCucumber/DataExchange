# DataExchange
using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Windows.Forms;

namespace CsvViewer
{
    public partial class Form1 : Form
    {
        private List<string[]> csvData = new List<string[]>();
        private int currentRowIndex = 0;

        public Form1()
        {
            InitializeComponent();
        }

        private void buttonHandUpload1_Click(object sender, EventArgs e)
        {
            OpenFileDialog dialog = new OpenFileDialog();
            dialog.Filter = "CSV files (*.csv)|*.csv";
            dialog.Multiselect = false;

            if (dialog.ShowDialog() == DialogResult.OK)
            {
                string path = dialog.FileName;
                csvData.Clear();
                currentRowIndex = 0;

                using (StreamReader reader = new StreamReader(new FileStream(path, FileMode.Open), Encoding.UTF8))
                {
                    string line;
                    while ((line = reader.ReadLine()) != null)
                    {
                        string[] fields = line.Split(',');
                        csvData.Add(fields);
                    }
                }

                DisplayCurrentRow();
            }
        }

        private void DisplayCurrentRow()
        {
            if (csvData.Count == 0 || currentRowIndex < 0 || currentRowIndex >= csvData.Count)
            {
                textBox1.Text = "";
                textBox2.Text = "";
                return;
            }

            string[] row = csvData[currentRowIndex];
            textBox1.Text = row.Length > 0 ? row[0] : "";
            textBox2.Text = row.Length > 1 ? row[1] : "";
        }

        private void buttonUp_Click(object sender, EventArgs e)
        {
            if (currentRowIndex > 0)
            {
                currentRowIndex--;
                DisplayCurrentRow();
            }
        }

        private void buttonDown_Click(object sender, EventArgs e)
        {
            if (currentRowIndex < csvData.Count - 1)
            {
                currentRowIndex++;
                DisplayCurrentRow();
            }
        }

        private void buttonLeft_Click(object sender, EventArgs e)
        {
            // Можно реализовать горизонтальное смещение по колонкам (если нужно)
        }

        private void buttonRight_Click(object sender, EventArgs e)
        {
            // Можно реализовать горизонтальное смещение по колонкам (если нужно)
        }

        private void buttonCopy1_Click(object sender, EventArgs e)
        {
            Clipboard.SetText(textBox1.Text);
        }

        private void buttonCopy2_Click(object sender, EventArgs e)
        {
            Clipboard.SetText(textBox2.Text);
        }
    }
}
