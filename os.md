# OS

| Function name | How to use |
| :--- | :--- |
| `os.getcwd()` | return current working directory |
| `os.chdir(path)` | change directory to `path` |
| `os.listdir(path='.')` | list files' names in the `path`\(`'.'` is the current directory and `'..'`is the parent directory\) |
| `os.mkdir(path)` | create single directory, if exists throw error `FileExistsError: [Errno 17] File exists: 'file_name'` |
| `os.makedirs(path)` | create multiple directory, if exists throw error `FileExistsError: [Errno 17] File exists: 'file_name/file_name'`Note: './a/b' and './a/c' do not conflict |
| `os.remove(path)` | delete file |
| `os.rmdir(path)` | delete single directory, throw error if not empty`OSError: [Errno 66] Directory not empty: 'path'` |
| `os.removedirs(path)` | delete multiple directory, throw error if not empty`OSError: [Errno 66] Directory not empty: 'path'` |
| `os.rename(old, new)` | rename |





