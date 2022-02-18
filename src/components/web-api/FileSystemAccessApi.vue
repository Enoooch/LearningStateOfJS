<template lang="pug">
h3 File System Access Api
button(@click="getTheFile()") Open a file & save it
button(@click="getDirectories()") Get relative path name
.result
</template>

<script setup>
/**
 * @description File System Access Api
 * @link https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API
 */

// Accessing files
// store a reference to our file handle
let fileHandle

async function getFile() {
  // open file picker
  ;[fileHandle] = await window.showOpenFilePicker()

  if (fileHandle.kind === 'file') {
    // run file code
  } else if (fileHandle.kind === 'directory') {
    // run directory code
  }
}

const pickerOpts = {
  types: [
    {
      description: 'Images',
      accept: {
        'image/*': ['.png', '.gif', '.jpeg', '.jpg'],
      },
    },
  ],
  excludeAcceptAllOption: true,
  multiple: false,
}
async function getTheFile() {
  // open file picker
  ;[fileHandle] = await window.showOpenFilePicker(pickerOpts)

  // get file contents
  const fileData = await fileHandle.getFile()

  // save file contents
  saveFile(fileData)
}

// Accessing directories
async function getDirectories() {
  let dirHandle = await window.showDirectoryPicker()
  // const dirName = 'directoryToGetName'
  // const currentDirHandle = dirHandle.getDirectoryHandle(dirName, { create: true })
  returnPathDirectories(dirHandle)
}

async function returnPathDirectories(directoryHandle) {
  // Get a file handle by showing a file picker:
  const [handle] = await self.showOpenFilePicker()
  if (!handle) {
    // User cancelled, or otherwise failed to open a file.
    return
  }

  // Check if handle exists inside directory our directory handle
  const relativePaths = await directoryHandle.resolve(handle)

  if (relativePaths === null) {
    // Not inside directory handle
  } else {
    // relativePaths is an array of names, giving the relative path

    for (const name of relativePaths) {
      // log each entry
      alert(name)
    }
  }
}

// Writing to files
async function saveFile(data) {
  // create a new handle
  const newHandle = await window.showSaveFilePicker()

  // create a FileSystemWritableFileStream to write to
  const writableStream = await newHandle.createWritable()

  // write our file
  await writableStream.write(data)

  // close the file and write the contents to disk.
  await writableStream.close()
}
</script>
