private void buttonSaveCsv_Click(object sender, EventArgs e)
{
    SaveFileDialog saveDialog = new SaveFileDialog();
    saveDialog.Filter = "CSV files (*.csv)|*.csv";
    saveDialog.DefaultExt = "csv";
    saveDialog.AddExtension = true;

    if (saveDialog.ShowDialog() == DialogResult.OK)
    {
        string path = saveDialog.FileName;

        using (StreamWriter writer = new StreamWriter(new FileStream(path, FileMode.Create), GetEncoding("windows-1251")))
        {
            foreach (var row in csvDataAutoUpload1)
            {
                string line = string.Join(";", row);
                writer.WriteLine(line);
            }
        }
    }
}
