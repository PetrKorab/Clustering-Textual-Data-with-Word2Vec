import streamlit as st
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
import json

# Authenticate with Google Drive
gauth = GoogleAuth()
gauth.LocalWebserverAuth()
drive = GoogleDrive(gauth)

# Function to list all JSON files in Google Drive
def list_json_files(folder_id):
    file_list = drive.ListFile({'q': f"'{folder_id}' in parents and trashed=false"}).GetList()
    json_files = [f for f in file_list if f['title'].endswith('.json')]
    return json_files

# Function to read JSON file content
def read_json_file(file_id):
    json_file = drive.CreateFile({'id': file_id})
    content = json.loads(json_file.GetContentString())
    return content

# Main function to run the Streamlit app
def main():
    st.title("JSON Files Reader from Google Drive")

    # Enter the folder ID where your JSON files are stored in Google Drive
    folder_id = st.text_input("Enter Google Drive Folder ID")

    if folder_id:
        json_files = list_json_files(folder_id)

        if json_files:
            st.write(f"Found {len(json_files)} JSON file(s):")

            for file in json_files:
                file_id = file['id']
                file_name = file['title']
                st.write(f"### {file_name}")

                content = read_json_file(file_id)
                st.json(content)
        else:
            st.write("No JSON files found in the specified folder.")

if __name__ == "__main__":
    main()
