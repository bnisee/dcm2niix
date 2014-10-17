Building command line version:

 This requires a C compiler. With a terminal, change directory to the 'conosle' folder and run the following: 
  g++ -O3 main_console.c nii_dicom.c nifti1_io_core.c nii_ortho.c nii_dicom_batch.c untgz.c -s -o dcm2niix -lz
Building command line version universal binary from OSX 64 bit system:
 This requires a C compiler. With a terminal, change directory to the 'conosle' folder and run the following: 
  g++ -O3 -lz -x c++ main_console.c nii_dicom.c nifti1_io_core.c nii_ortho.c nii_dicom_batch.c untgz.c -s -arch i386 -o dcm2niix32
  g++ -O3 -lz -x c++ main_console.c nii_dicom.c nifti1_io_core.c nii_ortho.c nii_dicom_batch.c untgz.c -s -o dcm2niix64
  lipo -create dcm2niix32 dcm2niix64 -o dcm2niix
 To validate that the resulting executable supports both architectures type
  file ./dcm2niix

Building OSX graphical user interface using XCode:
 Copy contents of "console" folder to /xcode/dcm2/core
 Open and compile "dcm2.xcodeproj" with XCode 4.6 or later
 
##### THE QT AND wxWIDGETS GUIs ARE NOT YET SUPPORT - FOLLOWING LINES FOR FUTURE VERSIONS ##### 
 
Building QT graphical user interface:
  Open "dcm2.pro" with QTCreator. This should work on OSX and Linux. On Windows the printf information is not redirected to the user interface 
  If compile gives you grief look at the .pro file which has notes for different operating systems.

Building using wxWidgets
wxWdigets makefiles are pretty complex and specific for your operating system. For simplicity, we will build the "clipboard" example that comes with wxwidgets and then substitute our own code. The process goes something like this.
 a.) Install wxwdigets
 b.) successfully "make" the samples/clipboard program
 c.) DELETE console/makefile. WE DO NOT WANT TO OVERWRITE the WX MAKEFILE 
 d.) with the exception of "makefile", copy the contents of console to /samples/clipboard
 e.) overwrite the original /samples/clipboard.cpp with the dcm2niix file of the same name
 f.) Older XCodes have problems with .cpp files, whereas wxWidgets's makefiles do not compile with "-x cpp". So the core files are called '.c' but we will rename them to .cpp for wxWidgets:
 rename 's/\.c$/\.cpp/' *
 g.) edit the /samples/clipboard makefile: Add "nii_dicom.o nifti1_io_core.o nii_ortho.o nii_dicom_batch.o \" to CLIPBOARD_OBJECTS:
CLIPBOARD_OBJECTS =  \
	nii_dicom.o nifti1_io_core.o nii_ortho.o nii_dicom_batch.o \
	$(__clipboard___win32rc) \
	$(__clipboard_os2_lib_res) \
	clipboard_clipboard.o
 h.) edit the /samples/clipboard makefile: With wxWidgets we will capture std::cout comments, not printf, so we need to add "-DDmyUseCOut" to CXXFLAGS:
CXXFLAGS = -DmyUseCOut -DWX_PRECOMP ....
 i.) For a full refresh
rm clipboard
rm *.o
make