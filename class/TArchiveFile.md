<!-- TArchiveFile.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 日 1月  6 13:34:45 2019 (+0800)
;; Last-Updated: 日 1月  6 13:40:43 2019 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 1
;; URL: http://wuhongyi.cn -->

# TArchiveFile

Class describing an archive file containing multiple sub-files, like a ZIP or TAR archive.

```cpp
class TArchiveFile : public TObject

TArchiveMember : public TObject
```

## class

**TArchiveFile**

```cpp
private:
   TArchiveFile(const TArchiveFile&);            ///< Not implemented because TArchiveFile can not be copied.
   TArchiveFile& operator=(const TArchiveFile&); ///< Not implemented because TArchiveFile can not be copied.

protected:
   TString         fArchiveName;  ///< Archive file name
   TString         fMemberName;   ///< Sub-file name
   Int_t           fMemberIndex;  ///< Index of sub-file in archive
   TFile          *fFile;         ///< File stream used to access the archive
   TObjArray      *fMembers;      ///< Members in this archive
   TArchiveMember *fCurMember;    ///< Current archive member

   static Bool_t ParseUrl(const char *url, TString &archive, TString &member, TString &type);
/// Try to determine if url contains an anchor specifying an archive member.
/// Returns kFALSE in case of an error.

public:
   TArchiveFile() : fArchiveName(""), fMemberName(""), fMemberIndex(-1), fFile(0), fMembers(0), fCurMember(0) { }
   TArchiveFile(const char *archive, const char *member, TFile *file);
/// Specify the archive name and member name.
/// \param[in] archive Name of the archive file
/// \param[in] member Name of the ROOT file or integer number
/// \param[in] file Address of the TFile instance from where the call takes place
/// The member can be a decimal
/// number which allows to access the n-th sub-file. This method is
/// normally only called via TFile.   
   
   virtual ~TArchiveFile();

   virtual Int_t   OpenArchive() = 0;
   virtual Int_t   SetCurrentMember() = 0;
   virtual Int_t   SetMember(const char *member);
/// Explicitely make the specified member the current member.
/// Returns -1 in case of error, 0 otherwise.   
   
   virtual Int_t   SetMember(Int_t idx);
/// Explicitely make the member with the specified index the current member.
/// Returns -1 in case of error, 0 otherwise.

   Long64_t        GetMemberFilePosition() const;/// Return position in archive of current member.
   TArchiveMember *GetMember() const { return fCurMember; }
   TObjArray      *GetMembers() const { return fMembers; }
   Int_t           GetNumberOfMembers() const;/// Returns number of members in archive.

   const char     *GetArchiveName() const { return fArchiveName; }
   const char     *GetMemberName() const { return fMemberName; }
   Int_t           GetMemberIndex() const { return fMemberIndex; }

   static TArchiveFile *Open(const char *url, TFile *file);
/// Return proper archive file handler depending on passed url.
/// The handler is loaded via the plugin manager and is triggered by
/// the extension of the archive file. In case no handler is found 0
/// is returned. The file argument is used to access the archive.
/// The archive should be specified as url with the member name as the
/// anchor, e.g. "root://pcsalo.cern.ch/alice/event_1.zip#tpc.root",
/// where tpc.root is the file in the archive to be opened.
/// Alternatively the sub-file can be specified via its index number,
/// e.g. "root://pcsalo.cern.ch/alice/event_1.zip#3".
/// This function is normally only called via TFile::Open().   
```

----

**TArchiveMember**

```cpp
friend class TArchiveFile;

protected:
   TString    fName;          ///< Name of member
   TString    fComment;       ///< Comment field
   TDatime    fModTime;       ///< Modification time
   Long64_t   fPosition;      ///< Byte position in archive
   Long64_t   fFilePosition;  ///< Byte position in archive where member data starts
   Long64_t   fCsize;         ///< Compressed size
   Long64_t   fDsize;         ///< Decompressed size
   Bool_t     fDirectory;     ///< Flag indicating this is a directory

public:
   TArchiveMember();
   TArchiveMember(const char *name);
   TArchiveMember(const TArchiveMember &member);
   TArchiveMember &operator=(const TArchiveMember &rhs);
   virtual ~TArchiveMember() { }

   const char *GetName() const { return fName; }
   const char *GetComment() const { return fComment; }
   TDatime     GetModTime() const { return fModTime; }
   Long64_t    GetPosition() const { return fPosition; }
   Long64_t    GetFilePosition() const { return fFilePosition; }
   Long64_t    GetCompressedSize() const { return fCsize; }
   Long64_t    GetDecompressedSize() const { return fDsize; }
   Bool_t      IsDirectory() const { return fDirectory; }
```




<!-- TArchiveFile.md ends here -->
