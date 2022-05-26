# JSON Cheatsheet
## JSON to Datagridview
### Data structure class
```
internal class Subject
  {
    public string EmployeeName = "aceng";
    public int EmployeeId = 2;
    public string EmployeeCity = "Boston";
    public string EmployeeStatus = "Termminated";
  }
```
### Using Newtonsoft Library and Declare the file path
```
using Newtonsoft.Json;
using System.IO;

string path = Directory.GetParent(Environment.CurrentDirectory).Parent.FullName + "\\database.json"; // get file path
```
### Deserialize (Read) JSON file and Insert to DataGridView
```

  private void ReadJSON()
  {
   
    string jsonString = File.ReadAllText(path);//stream the json file
    Subject databaseread = JsonConvert.DeserializeObject<Subject>(jsonString);//Invoke Data structure and deserialize (Read) the file
    
    dataGridView1.Rows.Clear();
    dataGridView1.Rows.Add();
    dataGridView1.Rows[0].Cells[0].Value = databaseread.EmployeeName;
    dataGridView1.Rows[0].Cells[1].Value = databaseread.EmployeeId;
    dataGridView1.Rows[0].Cells[2].Value = databaseread.EmployeeCity;
    dataGridView1.Rows[0].Cells[3].Value = databaseread.EmployeeStatus;
  }
```
### Serialize (Write) JSON file from DataGridView
```
private void WriteJSON()
{

    Subject Datasub = new Subject(); //invoke Data structure
    
    //First Get the value from DataGridView
    Datasub.EmployeeName =  dataGridView1.Rows[0].Cells[0].Value.ToString();
    Datasub.EmployeeId =  Convert.ToInt32(dataGridView1.Rows[0].Cells[1].Value);
    Datasub.EmployeeCity =  dataGridView1.Rows[0].Cells[2].Value.ToString();
    Datasub.EmployeeStatus =  dataGridView1.Rows[0].Cells[3].Value.ToString();
    
    //Then, Serialize the Data Structure
    string result = JsonConvert.SerializeObject(Datasub);
    
    //Write the Serialized datastructure to JSON file using StreamWriter
    if (File.Exists(path))
    {
        File.Delete(path);
        using (var tw = new StreamWriter(path, true))
        {
            tw.WriteLine(result.ToString());
            tw.Close();
        }
    }
    else if (!File.Exists(path))
    {
        using (var tw = new StreamWriter(path, true))
        {
            tw.WriteLine(result.ToString());
            tw.Close();
        }
    }
}
```
### Clear or nullify all key in JSON file
```
using Newtonsoft.Json;
using System.IO;

 private void ClearJSON()
 {
    // First, nullify the Data Structure
    Datasub.EmployeeName = "";
    Datasub.EmployeeId = 0;
    Datasub.EmployeeCity = "";
    Datasub.EmployeeStatus = "";
    
    // Then, Serialize (Write) the json file with the class
    string result = JsonConvert.SerializeObject(Datasub);
    
    //Using StreamWriter to modify the JSON key in file
    if (File.Exists(path))
    {
        File.Delete(path);
        using (var tw = new StreamWriter(path, true))
        {
            tw.WriteLine(result.ToString());
            tw.Close();
        }
    }
    else if (!File.Exists(path))
    {
        using (var tw = new StreamWriter(path, true))
        {
            tw.WriteLine(result.ToString());
            tw.Close();
        }
    }
}
```
