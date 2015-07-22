# Question4
# In-Memory File System
// Programming in C#

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace filesystem
{
    class Program
    {
        static void Main(string[] args)
        {
            string foldername = @"c:\homefolder";
        string pathString = System.IO.Path.Combine(foldername, "SubFolder");
        
	string sourcepath = @"c:\TestFolder";
	string destinationpath = @"TestFolder\SubDir";
	
	
	//string pathString2 = @"c:\homefolder\SubFolder2";
        
	// create the subfolder
	System.IO.Directory.CreateDirectory(pathString);
        
	// creat a file name
	string fileName = System.IO.Path.GetRandomFileName();

	// or create a file name
	// string fileName = "mynewfile.txt";
        
	// add the file name to the path
        pathString = System.IO.Path.Combine(pathString, fileName); 

	// show the path that we have constructed
	Console.WriteLine("Path to my file: {0}\n", pathString);
	
	// if the file does not exists, and create the file and write integers 0-99 
	if (!System.IO.File.Exists(pathString)) 
	{
		using (System.IO.FileStream fs = System.IO.File.Create(pathString))
		{
			for (byte i = 0; i<100; i++)
			{
				fs.WriteByte(i);
			}
		}
	}

	else
	{
		Console.WriteLine("File \"{0}\" already exists", fileName);
		return;
	}

	// read and show the data from my file
	try
	{
		byte[] readBuffer = System.IO.File.ReadAllBytes(pathString);
		foreach (byte b in readBuffer)
		{
			Console.Write(b + " ");
		}
	}
	catch (System.IO.IOException e)
	{
		Console.WriteLine(e.Message);
		
	}
	
	string sourcefile = System.IO.Path.Combine(sourcepath, fileName);
	string destinationfile = System.IO.Path.Combine(destinationpath, fileName);

	if (System.IO.Directory.CreateDirectory(destinationpath) == null)
	{
		System.IO.Directory.CreateDirectory(destinationpath);
	}
	
	System.IO.File.Copy(sourcefile, destinationfile, true);

	if (System.IO.Directory.Exists(sourcepath))
	{
		string[] files = System.IO.Directory.GetFiles(sourcepath);
		foreach (string s in files)
		{
			fileName = System.IO.Path.GetFileName(s);
			destinationfile = System.IO.Path.Combine(destinationpath, fileName);
			System.IO.File.Copy(s, destinationfile, true);
		}
	}
	else
	{
		Console.WriteLine("Source path does not exist");
	}




	// keep Console window open in debug mode
	System.Console.WriteLine("Press any key to exit");
	System.Console.ReadKey();
        }
    }
}
