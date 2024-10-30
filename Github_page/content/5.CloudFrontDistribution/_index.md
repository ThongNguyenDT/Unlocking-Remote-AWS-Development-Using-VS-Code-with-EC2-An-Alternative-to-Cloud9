import os

def rename_images(folder_path, new_name):
    files = os.listdir(folder_path)

    image_files = [f for f in files if f.lower().endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp'))]
    for i, filename in enumerate(image_files):
        ext = os.path.splitext(filename)[1]
        new_filename = f"{new_name}_{i + 1}{ext}"
        old_path = os.path.join(folder_path, filename)
        new_path = os.path.join(folder_path, new_filename)
        os.rename(old_path, new_path)
        print(f"Đã đổi tên '{filename}' thành '{new_filename}'")

folder_path = "path/to/your/folder"
new_name = "image"
rename_images(folder_path, new_name)