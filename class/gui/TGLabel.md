<!-- TGLabel.md --- 
;; 
;; Description: 
;; Author: Hongyi Wu(吴鸿毅)
;; Email: wuhongyi@qq.com 
;; Created: 二 11月  8 13:39:02 2016 (+0800)
;; Last-Updated: 二 11月  8 20:56:41 2016 (+0800)
;;           By: Hongyi Wu(吴鸿毅)
;;     Update #: 2
;; URL: http://wuhongyi.cn -->

# TGLabel

TGLabel 继承 TGFrame

## class

```cpp
   static FontStruct_t  GetDefaultFontStruct();
   static const TGGC   &GetDefaultGC();

   TGLabel(const TGWindow *p, TGString *text,
           GContext_t norm = GetDefaultGC()(),
           FontStruct_t font = GetDefaultFontStruct(),
           UInt_t options = kChildFrame,
           Pixel_t back = GetDefaultFrameBackground());
   TGLabel(const TGWindow *p = 0, const char *text = 0,
           GContext_t norm = GetDefaultGC()(),
           FontStruct_t font = GetDefaultFontStruct(),
           UInt_t options = kChildFrame,
           Pixel_t back = GetDefaultFrameBackground());

   virtual ~TGLabel();

   virtual TGDimension GetDefaultSize() const;

   const TGString *GetText() const { return fText; }
   virtual const char *GetTitle() const { return fText->Data(); }
   virtual void SetText(TGString *newText);
   void SetText(const char *newText) { SetText(new TGString(newText)); }
   virtual void ChangeText(const char *newText) { SetText(newText); } //*MENU*icon=bld_rename.png*
   virtual void SetTitle(const char *label) { SetText(label); }
   void  SetText(Int_t number) { SetText(new TGString(number)); }
   void  SetTextJustify(Int_t tmode);
   Int_t GetTextJustify() const { return fTMode; }
   virtual void SetTextFont(TGFont *font, Bool_t global = kFALSE);
   virtual void SetTextFont(FontStruct_t font, Bool_t global = kFALSE);
   virtual void SetTextFont(const char *fontName, Bool_t global = kFALSE);
   virtual void SetTextColor(Pixel_t color, Bool_t global = kFALSE);
   virtual void SetTextColor(TColor *color, Bool_t global = kFALSE);
   virtual void SetForegroundColor(Pixel_t fore) { SetTextColor(fore); }
   virtual void Disable(Bool_t on = kTRUE)
               { fDisabled = on; fClient->NeedRedraw(this); } //*TOGGLE* *GETTER=IsDisabled
   virtual void Enable() { fDisabled = kFALSE; fClient->NeedRedraw(this); }
   Bool_t IsDisabled() const { return fDisabled; }
   Bool_t HasOwnFont() const;

   void  SetWrapLength(Int_t wl) { fWrapLength = wl; Layout(); }
   Int_t GetWrapLength() const { return fWrapLength; }

   void  Set3DStyle(Int_t style) { f3DStyle = style; fClient->NeedRedraw(this); }
   Int_t Get3DStyle() const { return f3DStyle; }

   void SetMargins(Int_t left=0, Int_t right=0, Int_t top=0, Int_t bottom=0)
      { fMLeft = left; fMRight = right; fMTop = top; fMBottom = bottom; }
   Int_t GetLeftMargin() const { return fMLeft; }
   Int_t GetRightMargin() const { return fMRight; }
   Int_t GetTopMargin() const { return fMTop; }
   Int_t GetBottomMargin() const { return fMBottom; }

   GContext_t GetNormGC() const { return fNormGC; }
   FontStruct_t GetFontStruct() const { return fFont->GetFontStruct(); }
   TGFont      *GetFont() const  { return fFont; }

   virtual void Layout();
   virtual void SavePrimitive(std::ostream &out, Option_t *option = "");
```

## code

```cpp
#include "TGLabel.h"

// TGLabel
const char gReadyMsg[] = "Ready. You can drag list tree items to any \
pad in the canvas, or to the \"Base\" folder of the list tree itself...";
TGLabel *fStatus = new TGLabel(frame, new TGHotString(gReadyMsg));
fStatus->SetTextJustify(kTextLeft);
fStatus->SetTextColor(0x0000ff);
fStatus->Enable();
// fStatus->Disable();
// if (fStatus->IsDisabled()) ;
// fStatus->SetText("XXX");
// fStatus->SetText(125);
// fStatus->SetFont("XXX");
fStatus->SetText(Form("abc%ld",100);
frame->AddFrame(fStatus, new TGLayoutHints(kLHintsExpandX | kLHintsCenterY,10, 10, 10, 10));
```

## example



<!-- TGLabel.md ends here -->