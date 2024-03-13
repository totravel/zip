
## Introduction

A C++20 library offering a clean and simple interface to the miniz library for creating, reading, and updating ZIP files.

## Examples

```cpp
#include <iostream>
#include "zip.hpp"
#include "exceptions.hpp"

int main()
{
  try
  {
    minidocx::Zip z;

    // Create a new zip archive.
    z.open("foo.zip", minidocx::Zip::OpenMode::Create);
    z.addFileFromString("a.txt", "aaa");
    z.addFileFromString("dir/b.txt", "bbb");
    z.addFileFromString("dir/subdir/c.txt", "ccc");
    z.addFileFromString("dir/subdir/d.txt", "ddd");
    z.close();

    // Read the existing zip archive.
    z.open("foo.zip", minidocx::Zip::OpenMode::ReadOnly);
    std::cout << z.extractFileToString("a.txt") << std::endl;
    z.extractFileToDisk("dir/b.txt", "b.txt");
    z.close();

    // Update the existing zip archive.
    z.open("foo.zip", minidocx::Zip::OpenMode::Update);
    z.deleteFiles({ "a.txt", "dir/subdir/c.txt" });
    z.addFileFromDisk("readme.md", "README.md");
    z.close();
  }
  catch (const minidocx::exception& ex)
  {
    std::cerr << ex.what() << std::endl;
  }

  return 0;
}
```

Note:

- No need to add directories manually unless you really want to add an empty directory. In this case, call the `create_directory()` method with an directory name ending in a forwardslash `/`.
- If you delete a directory, it will also delete all files and directories in that directory.
