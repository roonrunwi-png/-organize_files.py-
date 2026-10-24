    # -organize_files.py-python organize_files.py
##run
    from pathlib import Path
    import shutil

    FILE_CATEGORIES = {
    "images": {".jpg", ".jpeg", ".png", ".gif", ".webp", ".heic"},
    "documents": {".pdf", ".doc", ".docx", ".xls", ".xlsx", ".csv", ".ppt", ".pptx", ".txt"},
    "videos": {".mp4", ".mov", ".avi", ".mkv"},
    "audio": {".mp3", ".wav", ".m4a", ".flac"},
    "archives": {".zip", ".rar", ".7z", ".tar", ".gz"},
}

    def get_category(file_extension: str) -> str:
    """คืนชื่อหมวดหมู่ของไฟล์ตามนามสกุล"""
    for category, extensions in FILE_CATEGORIES.items():
        if file_extension.lower() in extensions:
            return category
    return "others"

def organize_files(source_folder: str) -> None:
    """จัดเก็บไฟล์ในโฟลเดอร์ต้นทางลงโฟลเดอร์ย่อยตามประเภท"""
    source_path = Path(source_folder)

    if not source_path.exists():
        raise FileNotFoundError(f"ไม่พบโฟลเดอร์: {source_path}")

    for item in source_path.iterdir():
        if not item.is_file():
            continue

        category = get_category(item.suffix)
        destination_folder = source_path / category
        destination_folder.mkdir(exist_ok=True)

        destination_file = destination_folder / item.name

        # ถ้ามีไฟล์ชื่อเดิมอยู่แล้ว ให้เติมเลขท้าย
        counter = 1
        while destination_file.exists():
            destination_file = destination_folder / f"{item.stem}_{counter}{item.suffix}"
            counter += 1

        shutil.move(str(item), str(destination_file))
        print(f"ย้าย: {item.name} -> {category}/")

if __name__ == "__main__":
    organize_files("c download files ")  # เปลี่ยนเป็นพาธโฟลเดอร์ที่ต้องการ
