── main.py (Main Python file with GUI)
└── sample.csv (Sample CSV file for testing)

Project structure
import tkinter as tk
from tkinter import filedialog, messagebox
import pandas as pd

class DataAnalysisApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Data Analysis App")
        self.root.geometry("600x400")
        
        self.file_path = None
        self.data = None
        
        # Create UI components
        self.create_widgets()
        
    def create_widgets(self):
        # Upload CSV button
        upload_button = tk.Button(self.root, text="Upload CSV File", command=self.upload_file)
        upload_button.pack(pady=10)
        
        # Label for displaying selected file
        self.file_label = tk.Label(self.root, text="No file selected", fg="gray")
        self.file_label.pack(pady=5)
        
        # Analysis buttons
        analyze_button = tk.Button(self.root, text="Analyze Data", command=self.analyze_data)
        analyze_button.pack(pady=10)
        
        # Results area
        self.result_text = tk.Text(self.root, height=10, width=60)
        self.result_text.pack(pady=10)
        
    def upload_file(self):
        # File dialog to choose CSV file
        self.file_path = filedialog.askopenfilename(filetypes=[("CSV files", "*.csv")])
        
        if self.file_path:
            self.file_label.config(text=f"Selected File: {self.file_path}")
            try:
                self.data = pd.read_csv(self.file_path)
                messagebox.showinfo("File Upload", "File uploaded successfully!")
            except Exception as e:
                messagebox.showerror("Error", f"Failed to load file: {str(e)}")
                self.file_label.config(text="No file selected")
        else:
            self.file_label.config(text="No file selected")
    
    def analyze_data(self):
        if self.data is None:
            messagebox.showwarning("No Data", "Please upload a CSV file first.")
            return
        
        # Perform analysis
        try:
            # Getting basic statistics
            description = self.data.describe().to_string()
            
            # Getting column names
            columns = ", ".join(self.data.columns.tolist())
            
            # Displaying results
            self.result_text.delete(1.0, tk.END)
            self.result_text.insert(tk.END, f"Columns: {columns}\n\n")
            self.result_text.insert(tk.END, f"Data Description:\n{description}\n")
        except Exception as e:
            messagebox.showerror("Error", f"Error analyzing data: {str(e)}")
    
def main():
    root = tk.Tk()
    app = DataAnalysisApp(root)
    root.mainloop()

if __name__ == "__main__":
    main()

