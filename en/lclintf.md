# lclintf

**LCLIntf** is a Lazarus unit that provides compatibility with Delphi in a cross-platform way. It also contains some additional functions 

On defining a new [package](<Lazarus_Packages.md> "Lazarus Packages"), make sure that [LCL](<LCL.md> "LCL") is added as requirement to get rid of 'missing LCLIntf' error. 

## functions and procedures

It can replace code in the (Delphi) Windows unit. 
    
    
     function PredefinedClipboardFormat(AFormat: TPredefinedClipboardFormat): TClipboardFormat;
     function MsgKeyDataToShiftState(KeyData: PtrInt): TShiftState;
     function GetTickCount: DWord; inline;
     function GetTickCount64: QWord; inline;
     function FindDefaultBrowser(out ABrowser, AParams: String): Boolean;
     function OpenURL(AURL: String): Boolean;
     function OpenDocument(APath: String): Boolean;
    

## winapih.inc
    
    
     function Arc(DC: HDC; Left,Top,Right,Bottom,Angle16Deg, Angle16DegLength: Integer): Boolean;
     function AngleChord(DC: HDC; x1,y1,x2,y2,angle1,angle2 : Integer): Boolean;
     function BeginPaint(Handle : hwnd; var PS : TPaintStruct): hdc;
     function BitBlt(DestDC: HDC; X, Y, Width, Height: Integer; SrcDC: HDC; XSrc, YSrc: Integer; Rop: DWORD): Boolean; 
     function CallNextHookEx(hhk : HHOOK; ncode : Integer; WParam: WParam; LParam: LParam) : Integer;
     function CallWindowProc(lpPrevWndFunc : TFarProc; Handle : HWND; Msg : UINT; WParam: WParam; LParam: LParam): Integer;
     //function CharLower --> independent
     //function CharLowerBuff --> independent
     //function CharUpper --> independent
     function ClientToScreen(Handle : HWND; var P : TPoint) : Boolean;
     function CombineRgn(Dest, Src1, Src2 : HRGN; fnCombineMode : Longint) : Longint;
     function CreateBitmap(Width, Height: Integer; Planes, BitCount: Longint; BitmapBits: Pointer): HBITMAP;
     function CreateBrushIndirect(const LogBrush: TLogBrush): HBRUSH;
     function CreateCaret(Handle: HWND; Bitmap: hBitmap; width, Height: Integer): Boolean;
     function CreateCompatibleBitmap(DC: HDC; Width, Height: Integer): HBITMAP;
     function CreateCompatibleDC(DC: HDC): HDC;
     function CreateDIBitmap(DC: HDC; var InfoHeader: TBitmapInfoHeader;
                             dwUsage: DWORD; InitBits: PChar; var InitInfo: TBitmapInfo;
                             wUsage: UINT): HBITMAP;
     function CreateDIBSection(DC: HDC; const BitmapInfo: tagBitmapInfo; Usage: UINT;
                               var Bits: Pointer; SectionHandle: THandle; Offset: DWORD): HBITMAP;
     function CreateEllipticRgn(X1, Y1, X2, Y2: Integer): HRGN;
     //function CreateEllipticRgnIndirect --> independent
     //function CreateFont --> independent
     function CreateFontIndirect(const LogFont: TLogFont): HFONT;
     function CreateFontIndirectEx(const LogFont: TLogFont; const LongFontName: string): HFONT;
     function CreateIconIndirect(IconInfo: PIconInfo): HICON;
     function CreatePalette(const LogPalette: TLogPalette): HPalette;
     function CreatePatternBrush(ABitmap: HBITMAP): HBRUSH;
     //function CreatePen --> independent
     function CreatePenIndirect(const LogPen: TLogPen): HPEN;
     function CreatePolygonRgn(Points: PPoint; NumPts: Integer; FillMode: integer): HRGN;
     function CreateRectRgn(X1,Y1,X2,Y2 : Integer): HRGN;
     //function CreateRectRgnIndirect --> independent
     function CreateRoundRectRgn(X1, Y1, X2, Y2, nWidthEllipse, nHeightEllipse: Integer): HRGN;  
     procedure DeleteCriticalSection(var CritSection: TCriticalSection);
     function DeleteDC(hDC: HDC): Boolean;
     function DeleteObject(GDIObject: HGDIOBJ): Boolean;
     function DestroyCaret(Handle : HWND): Boolean;
     function DestroyCursor(Handle: HCURSOR): Boolean;
     function DestroyIcon(Handle: HICON): Boolean;
     function DPtoLP(DC: HDC; var Points; Count: Integer): BOOL;
     function DrawFrameControl(DC: HDC; const Rect : TRect; uType, uState : Cardinal) : Boolean;
     function DrawFocusRect(DC: HDC; const Rect: TRect): Boolean;
     function DrawEdge(DC: HDC; var Rect: TRect; edge: Cardinal; grfFlags: Cardinal): Boolean;
     function DrawText(DC: HDC; Str: PChar; Count: Integer; var Rect: TRect; Flags: Cardinal): Integer; 
     function EnableScrollBar(Wnd: HWND; wSBflags, wArrows: Cardinal): Boolean;
     function EnableWindow(hWnd: HWND; bEnable: Boolean): Boolean;
     function EndPaint(Handle : hwnd; var PS : TPaintStruct): Integer;
     procedure EnterCriticalSection(var CritSection: TCriticalSection);
     function EnumDisplayMonitors(hdc: HDC; lprcClip: PRect; lpfnEnum: MonitorEnumProc; dwData: LPARAM): LongBool;
     function EnumFontFamilies(DC: HDC; Family: Pchar; EnumFontFamProc: FontEnumProc; LParam:Lparam):longint;
     function EnumFontFamiliesEx(DC: HDC; lpLogFont:PLogFont; Callback: FontEnumExProc; Lparam:LParam; Flags: dword): longint;  
     function Ellipse(DC: HDC; x1, y1, x2, y2: Integer): Boolean;
     //function EqualRect --> independent
     function EqualRgn(Rgn1: HRGN; Rgn2: HRGN): Boolean;
     function ExcludeClipRect(dc: hdc; Left, Top, Right, Bottom : Integer) : Integer;
     function ExtCreatePen(dwPenStyle, dwWidth: DWord; const lplb: TLogBrush; dwStyleCount: DWord; lpStyle: PDWord): HPEN;
     function ExtTextOut(DC: HDC; X, Y: Integer; Options: Longint; Rect: PRect; Str: PChar; Count: Longint; Dx: PInteger): Boolean;
     function ExtSelectClipRGN(dc: hdc; rgn : hrgn; Mode : Longint) : Integer; 
     function FillRect(DC: HDC; const Rect: TRect; Brush: HBRUSH): Boolean;
     function FillRgn(DC: HDC; RegionHnd: HRGN; hbr: HBRUSH): BOOL;
     function FloodFill(DC: HDC; X, Y: Integer;
                        Color: TGraphicsColor;
                        FillStyle: TGraphicsFillStyle;
                        Brush: HBRUSH): Boolean;
     function FrameRect(DC: HDC; const ARect: TRect; hBr: HBRUSH): Integer; 
     function GetActiveWindow : HWND;
     function GetBitmapBits(Bitmap: HBITMAP; Count: Longint;  Bits: Pointer): Longint;
     function GetBkColor(DC: HDC): TColorRef;
     function GetCapture : HWND;
     function GetCaretPos(var lpPoint: TPoint): Boolean;
     function GetClientRect(handle : HWND; var Rect: TRect) : Boolean;
     function GetClipBox(DC : hDC; lpRect : PRect) : Longint;
     function GetClipRGN(DC : hDC; RGN : hRGN) : Longint;
     function GetCurrentObject(DC: HDC; uObjectType: UINT): HGDIOBJ;
     function GetCursorPos(var lpPoint: TPoint): Boolean;
     function GetCharABCWidths(DC: HDC; p2, p3: UINT; const ABCStructs): Boolean;  //pbd
     function GetDC(hWnd: HWND): HDC;
     function GetDeviceCaps(DC: HDC; Index: Integer): Integer;
     function GetDIBits(DC: HDC; Bitmap: HBitmap; StartScan, NumScans: UINT; Bits: Pointer; var BitInfo: BitmapInfo; Usage: UINT):  Integer;
     function GetDoubleClickTime: UINT;
     function GetFocus: HWND;
     function GetFontLanguageInfo(DC: HDC): DWord;
     function GetForegroundWindow: HWND;
     function GetIconInfo(AIcon: HICON; AIconInfo: PIconInfo): Boolean;
     function GetKeyState(nVirtKey: Integer): Smallint;
     function GetMapMode(DC: HDC): Integer;
     function GetMonitorInfo(hMonitor: HMONITOR; lpmi: PMonitorInfo): Boolean;
     function GetObject(GDIObject: HGDIOBJ; BufSize: Integer; Buf: Pointer): Integer;
     function GetPaletteEntries(Palette: HPALETTE; StartIndex, NumEntries: UINT; var PaletteEntries): UINT;
     function GetParent(Handle : HWND): HWND;
     function GetProp(Handle : hwnd; Str : PChar): Pointer;
     function GetRgnBox(RGN : HRGN; lpRect : PRect) : Longint;
     function GetROP2(DC: HDC): integer;
     function GetScrollInfo(Handle: HWND; SBStyle: Integer; var ScrollInfo: TScrollInfo): Boolean;
     //function GetScrollPos --> independent
     //function GetScrollRange --> independent
     function GetStockObject(Value: Integer): THandle;
     function GetSysColor(nIndex: Integer): DWORD;
     function GetSysColorBrush(nIndex: Integer): HBrush;
     function GetSystemPaletteEntries(DC: HDC; StartIndex, NumEntries: UINT; var PaletteEntries): UINT;
     function GetSystemMetrics(nIndex: Integer): Integer;
     function GetTextColor(DC: HDC) : TColorRef;
     function GetTextExtentExPoint(DC: HDC; Str: PChar; Count, MaxWidth: Integer; MaxCount, PartialWidths: PInteger; var Size: TSize): boolean;
     function GetTextExtentPoint(DC: HDC; Str: PChar; Count: Integer; var Size: TSize): Boolean;
     function GetTextExtentPoint32(DC: HDC; Str: PChar; Count: Integer; var Size: TSize): Boolean;
     function GetTextMetrics(DC: HDC; var TM: TTextMetric): Boolean; 
     function GetViewPortExtEx(DC: HDC; Size: PSize): Integer;
     function GetViewPortOrgEx(DC: HDC; P: PPoint): Integer;
     function GetWindowExtEx(DC: HDC; Size: PSize): Integer;
     function GetWindowLong(Handle : hwnd; int : Integer): PtrInt;
     function GetWindowRect(Handle : hwnd; var Rect : TRect): Integer;
     function GetWindowSize(Handle : hwnd; var Width, Height: integer): boolean;
     function GetWindowOrgEx(dc : hdc; var P: TPoint): Integer;  // because of delphi compatibility
     function GetWindowOrgEx(dc : hdc; P : PPoint): Integer;  // an old declaration
     function GradientFill(DC: HDC; Vertices: PTriVertex; NumVertices : Longint; Meshes: Pointer; NumMeshes : Longint; Mode : Longint): Boolean; 
     function HideCaret(hWnd: HWND): Boolean;
     //function IndexToOverlayMask --> independent
     //function InflateRect --> independent
     procedure InitializeCriticalSection(var CritSection: TCriticalSection);
     function IntersectClipRect(dc: hdc; Left, Top, Right,Bottom: Integer): Integer;
     //function IntersectRect --> independent
     function InvalidateRect(aHandle : HWND; ARect : pRect; bErase : Boolean) : Boolean;
     function InvalidateRgn(Handle: HWND; Rgn: HRGN; Erase: Boolean): Boolean;
     //function IsCharAlphaNumeric --> independent
     function IsDBCSLeadByte(TestChar: Byte): boolean;
     //function IsRectEmpty --> independent
     function IsIconic(handle: HWND): boolean;
     function IsWindow(handle: HWND): boolean;
     function IsWindowEnabled(handle: HWND): boolean;
     function IsWindowVisible(handle: HWND): boolean;
     function IsZoomed(handle: HWND): boolean; 
     //function KillTimer --> independent
     procedure LeaveCriticalSection(var CritSection: TCriticalSection);
     function LineTo(DC: HDC; X, Y: Integer): Boolean;
     function LoadBitmap(hInstance: THandle; lpBitmapName: PChar): HBitmap;
     function LoadCursor(hInstance: THandle; lpCursorName: PChar): HCursor;
     function LoadIcon(hInstance: THandle; lpIconName: PChar): HIcon;
     function LPtoDP(DC: HDC; var Points; Count: Integer): BOOL; 
     //function MakeLong --> independent
     //function MakeROP4 --> independent
     //function MakeWord --> independent
     function MaskBlt(DestDC: HDC; X, Y, Width, Height: Integer; SrcDC: HDC; XSrc, YSrc: Integer; Mask: HBITMAP; XMask, YMask: Integer;  Rop: DWORD): Boolean;
     function MaskBlt(DestDC: HDC; X, Y, Width, Height: Integer; SrcDC: HDC; XSrc, YSrc: Integer; Mask: HBITMAP; XMask, YMask: Integer): Boolean;
     function MessageBox(hWnd: HWND; lpText, lpCaption: PChar; uType: Cardinal): integer;
     function MonitorFromPoint(ptScreenCoords: TPoint; dwFlags: DWord): HMONITOR;
     function MonitorFromRect(lprcScreenCoords: PRect; dwFlags: DWord): HMONITOR;
     function MonitorFromWindow(hWnd: HWND; dwFlags: DWord): HMONITOR; 
     function MoveToEx(DC: HDC; X, Y: Integer; OldPoint: PPoint): Boolean;
     //function OffsetRect --> independent
     function OffsetRgn(RGN: HRGN; nXOffset, nYOffset: Integer): Integer;
     function PaintRgn(DC: HDC; RGN: HRGN): Boolean;
     function PeekMessage(var lpMsg : TMsg; Handle : HWND; wMsgFilterMin, wMsgFilterMax,wRemoveMsg : UINT): Boolean;
     function Pie(DC: HDC; x1, y1, x2, y2, sx, sy, ex, ey: Integer): Boolean;
     function PolyBezier(DC: HDC; Points: PPoint;  NumPts: Integer; Filled, Continuous: boolean): boolean;
     function Polygon(DC: HDC; Points: PPoint;  NumPts: Integer; Winding: boolean): boolean;
     function Polyline(DC: HDC; Points: PPoint; NumPts: Integer): boolean;
     function PostMessage(Handle: HWND; Msg: Cardinal; WParam: WParam; LParam: LParam): Boolean;
     function PtInRegion(RGN: HRGN; X, Y: Integer): Boolean;
     //function PtInRect --> independent 
     function RealizePalette(DC: HDC): Cardinal;
     function Rectangle(DC: HDC; X1, Y1, X2, Y2: Integer): Boolean;
     function RectInRegion(RGN: HRGN; ARect: TRect): Boolean;
     function RectVisible(DC: HDC; const ARect: TRect): Boolean;
     function RedrawWindow(Wnd: HWND; lprcUpdate: PRECT; hrgnUpdate: HRGN; flags: UINT): Boolean;
     function ReleaseCapture : Boolean;
     function ReleaseDC(hWnd: HWND; DC: HDC): Integer;
     function RemoveProp(Handle: hwnd; Str: PChar): THandle;
     function RestoreDC(DC: HDC; SavedDC: Integer): Boolean;
     function RoundRect(DC : hDC; X1, Y1, X2, Y2: Integer; RX,RY : Integer): Boolean; 
     function SaveDC(DC: HDC): Integer;
     function ScreenToClient(Handle : HWND; var P : TPoint) : Integer;
     //function ScrollWindow --> independent deprecated calls ScrollWindowEx
     function ScrollWindowEx(hWnd: HWND; dx, dy: Integer; prcScroll, prcClip: PRect; hrgnUpdate: HRGN; prcUpdate: PRect; flags: UINT): Boolean;
     function SelectClipRGN(DC : hDC; RGN : HRGN) : Longint;
     function SelectObject(DC: HDC; GDIObj: HGDIOBJ): HGDIOBJ;
     function SelectPalette(DC: HDC; Palette: HPALETTE; ForceBackground: Boolean): HPALETTE;
     function SendMessage(HandleWnd: HWND; Msg: Cardinal; WParam: WParam; LParam: LParam): LResult;
     function SetActiveWindow(Handle: HWND): HWND;
     function SetBkColor(DC: HDC; Color: TColorRef): TColorRef;  //pbd
     function SetBkMode(DC: HDC; bkMode : Integer): Integer;
     function SetCapture (AHandle: HWND): HWND;
     function SetCaretPos(X, Y: Integer): Boolean;
     function SetCaretPosEx(handle: HWnd; X, Y: Integer): Boolean;
     function SetCursor(hCursor: HICON): HCURSOR;
     function SetCursorPos(X, Y: Integer): Boolean;
     function SetFocus(hWnd: HWND): HWND;
     function SetForegroundWindow(hWnd : HWND): Boolean;
     function SetMapMode(DC: HDC; fnMapMode : Integer): Integer;
     function SetMenu(AWindowHandle: HWND; AMenuHandle: HMENU): Boolean;
     function SetParent(hWndChild: HWND; hWndParent: HWND): HWND;
     function SetProp(Handle: hwnd; Str : PChar; Data : Pointer) : Boolean;
     //function SetRect --> independent
     function SetRectRgn(aRGN: HRGN; X1, Y1, X2, Y2 : Integer): Boolean;
     //function SetRectEmpty --> independent
     function SetROP2(DC: HDC; Mode: Integer): Integer;
     function SetScrollInfo(Handle: HWND; SBStyle: Integer; ScrollInfo: TScrollInfo; Redraw : Boolean): Integer;
     //function SetScrollPos --> independent
     //function SetScrollRange --> independent
     function SetStretchBltMode(DC: HDC; StretchMode: Integer): Integer;
     function SetSysColors(cElements: Integer; const lpaElements; const lpaRgbValues): Boolean;
     function SetTextCharacterExtra(_hdc : hdc; nCharExtra : Integer):Integer;
     function SetTextColor(DC: HDC; Color: TColorRef): TColorRef;
     //function SetTimer --> independent
     function SetWindowLong(Handle: HWND; Idx: Integer; NewLong : PtrInt): PtrInt;
     function SetViewPortExtEx(DC: HDC; XExtent, YExtent : Integer; OldSize: PSize): Boolean;
     function SetViewPortOrgEx(DC: HDC; NewX, NewY: Integer; OldPoint: PPoint): Boolean;
     function SetWindowExtEx(DC: HDC; XExtent, YExtent: Integer; OldSize: PSize): Boolean;
     function SetWindowOrgEx(dc : hdc; NewX, NewY: Integer; OldPoint: PPoint): Boolean;
     function SetWindowPos(hWnd: HWND; hWndInsertAfter: HWND;
            X, Y, cx, cy: Integer; uFlags: UINT): Boolean;
     function SetWindowRgn(hWnd: HWND; hRgn: HRGN; bRedraw: Boolean): longint;
     function ShowCaret(hWnd: HWND): Boolean;
     function ShowScrollBar(Handle: HWND; wBar: Integer; bShow: Boolean): Boolean;
     function ShowWindow(hWnd: HWND; nCmdShow: Integer): Boolean;
     function StretchBlt(DestDC: HDC; X, Y, Width, Height: Integer; SrcDC: HDC; XSrc, YSrc, SrcWidth, SrcHeight: Integer; Rop:  Cardinal): Boolean ;
     function StretchDIBits(DC: HDC; DestX, DestY, DestWidth, DestHeight, SrcX,
       SrcY, SrcWidth, SrcHeight: Integer; Bits: Pointer; var BitsInfo: TBitmapInfo;
       Usage: UINT; Rop: DWORD): Integer;
     //function SubtractRect --> independent
     function SystemParametersInfo(uiAction: DWord; uiParam: DWord; pvParam: Pointer; fWinIni: DWord): LongBool;  
     function TextOut(DC: HDC; X,Y : Integer; Str : Pchar; Count: Integer) : Boolean;
     function UpdateWindow(Handle: HWND): Boolean;
     //function UnionRect --> independent 
     function WindowFromPoint(Point : TPoint) : HWND;
    

## lclintfh.inc

Platform independent 
    
    
     function AdjustWindowRectEx(Var Rect: TRect; Style1: Word; MenuExist : Boolean; Style2 : Word) : Boolean;
     function CharLower(c: char): char; inline;
     function CharLowerBuff(pStr : PChar; Len : Integer): Integer;
     function CharUpper(c: char): char; inline;
     function CharUpperBuff(pStr : PChar; Len : Integer): Integer;
     function CopyRect(var DestRect: TRect; const SrcRect: TRect): Boolean;
     function CreateEllipticRgnIndirect(const ARect: TRect): HRGN;
     function CreateFont(Height, Width, Escapement, Orientation, Weight: Integer;
       Italic, Underline, StrikeOut, CharSet, OutputPrecision, ClipPrecision,
       Quality, PitchAndFamily: Byte; FaceName: PChar): HFONT;
     function CreatePen(Style, Width: Integer; Color: TColorRef): HPEN;
     function CreateRectRgnIndirect(const ARect: TRect): HRGN;
     function CreateSolidBrush(Color: TColorRef): HBRUSH;
     function EqualRect(const lprc1, lprc2: TRect): Boolean;
     function GetScrollPos(Handle: HWND; nBar: Integer): Integer;
     function GetScrollRange(Handle: HWND; nBar: Integer; var lpMinPos, lpMaxPos: Integer): Boolean;
     function GetRValue(RGB : DWORD) : BYTE; inline;
     function GetGValue(RGB : DWORD) : BYTE; inline;
     function GetBValue(RGB : DWORD) : BYTE; inline;
     procedure GetRGBValues(RGB : DWORD; out R, G, B: Byte); inline;
     procedure GetRGBIntValues(RGB : DWORD; out R, G, B: integer); inline; 
     function IndexToOverlayMask(iOverlay: UINT): UINT; inline;
     function InflateRect(var ARect: TRect; dx, dy: Integer): Boolean;
     function IntersectRect(var DestRect: TRect; const SrcRect1, SrcRect2: TRect): Boolean;
     function IsCharAlphaNumeric(c : Char) : Boolean;
     function IsRectEmpty(const ARect: TRect): Boolean; 
     function KillTimer(Wnd: HWND; uIDEvent: UINT_PTR): BOOL;
     function MakeLong(A,B: Word): DWORD; inline;
     function MakeROP4(fore, back: DWORD): DWORD;inline;
     function MakeWord(A,B: Byte): Word; inline;
     function MakeWParam(l, h: Word): WPARAM; inline;
     function MakeLParam(l, h: Word): LPARAM; inline;
     function MakeLResult(l, h: Word): LRESULT; inline; 
     function OffsetRect(var Rect: TRect; dx,dy: Integer): Boolean; inline; 
     function PtInRect(const Rect : TRect; const Point : TPoint) : Boolean; inline;
     function PointToSmallPoint(const P : TPoint) : TSmallPoint; inline; 
     function RGB(R, G, B : Byte) : TColorRef; inline;
     function ScrollWindow(hWnd: HWND; XAmount, YAmount: Integer; Rect, ClipRect: PRect): Boolean; inline;
     function SetRect(var ARect : TRect; xLeft,yTop,xRight,yBottom : Integer) : Boolean;
     function SetRectEmpty(var ARect: TRect): Boolean;
     function SetScrollPos(Handle: HWND; nBar, nPos: Integer; bRedraw: Boolean): Integer;
     function SetScrollRange(Handle: HWND; nBar, nMinPos, nMaxPos: Integer; bRedraw: Boolean): Boolean;
     function SetTimer(Wnd: HWND; nIDEvent: UINT_PTR; uElapse: UINT;  lpTimerFunc: TTimerProc): UINT_PTR;
     function SmallPointToPoint(const P : TSmallPoint) : Tpoint;
     function SubtractRect(out lprcDst: TRect; const lprcSrc1, lprcSrc2: TRect): Boolean; 
     function UnionRect(var DestRect: TRect; const SrcRect1, SrcRect2: TRect): Boolean; //pbd
    
    Platform specific stuff
     function AddEventHandler(AHandle: THandle; AFlags: dword; AEventHandler: TWaitHandleEvent; AData: PtrInt): PEventHandler;
     function AddProcessEventHandler(AHandle: THandle; AEventHandler: TChildExitEvent; AData: PtrInt): PProcessEventHandler;
     function AddPipeEventHandler(AHandle: THandle; AEventHandler: TPipeEvent; AData: PtrInt): PPipeEventHandler;
     function AllocateHWnd(Method: TLCLWndMethod): HWND;
     function AskUser(const DialogCaption, DialogMessage: string; DialogType: LongInt; Buttons: TDialogButtons; HelpCtx: Longint): LongInt;
      // ! ClipboardGetFormats: List will be created. You must free it yourself with FreeMem(List) !
     procedure CallDefaultWndHandler(Sender: TObject; var Message);
     function ClipboardFormatToMimeType(FormatID: TClipboardFormat): string;
     function ClipboardGetData(ClipboardType: TClipboardType; FormatID: TClipboardFormat; Stream: TStream): boolean;
     function ClipboardGetFormats(ClipboardType: TClipboardType; var Count: integer; var List: PClipboardFormat): boolean;
     function ClipboardGetOwnerShip(ClipboardType: TClipboardType; OnRequestProc: TClipboardRequestEvent;  FormatCount: integer; Formats: PClipboardFormat): boolean;
     function ClipboardRegisterFormat(const AMimeType: string): TClipboardFormat;
     function CreateEmptyRegion: hRGN;
     function CreatePixmapIndirect(const Data: Pointer; const TransColor: Longint): HBITMAP;
     function CreateRegionCopy(SrcRGN: hRGN): hRGN;
     function CreateRubberBand(const ARect: TRect; const ABrush: HBrush = 0): HWND;
     function CreateStandardCursor(ACursor: SmallInt): hCursor;
     function DCClipRegionValid(DC: HDC): boolean;
     procedure DeallocateHWnd(Wnd: HWND);
     procedure DestroyRubberBand(ARubberBand: HWND);
     procedure DrawDefaultDockImage(AOldRect, ANewRect: TRect; AOperation: TDockImageOperation);
     procedure DrawGrid(DC: HDC; const R: TRect; DX, DY: Integer);
     function ExtUTF8Out(DC: HDC; X, Y: Integer; Options: Longint; Rect: PRect; Str: PChar; Count: Longint; Dx: PInteger): Boolean;
     function FontCanUTF8(Font: HFont): boolean;  deprecated;
     function FontIsMonoSpace(Font: HFont): boolean;
     function Frame3d(DC: HDC; var ARect: TRect; const FrameWidth : integer; const Style : TGraphicsBevelCut): Boolean;
     function GetAcceleratorString(const AVKey: Byte; const AShiftState: TShiftState): String;
     function GetAvailableNativeCanvasTypes(DC: HDC; AAllowFallbackToParent: Boolean = False): TNativeCanvasTypes;
     function GetAvailableNativeHandleTypes(Handle: HWND; AAllowFallbackToParent: Boolean = False): TNativeHandleTypes;
     function GetCaretRespondToFocus(handle: HWND; var ShowHideOnFocus: boolean): Boolean;
     function GetClientBounds(handle : HWND; var ARect: TRect) : Boolean;
     function GetCmdLineParamDescForInterface: string;
     function GetControlConstraints(Constraints: TObject): boolean;
     function GetDCOriginRelativeToWindow(PaintDC: HDC; WindowHandle: HWND; var OriginDiff: TPoint): boolean;
     function GetDesignerDC(WindowHandle: HWND): HDC;
     function GetDeviceSize(DC: HDC; var p: TPoint): boolean;
     function GetLCLOwnerObject(Handle: HWnd): TObject;
     function GetNativeCanvas(DC: HDC; AHandleType: TNativeCanvasType; AAllowFallbackToParent: Boolean = False): PtrInt;
     function GetNativeHandle(Handle: HWND; AHandleType: TNativeHandleType; AAllowFallbackToParent: Boolean = False): PtrInt;
     function GetScrollBarSize(Handle: HWND; SBStyle: Integer): integer;
     function GetScrollbarVisible(Handle: HWND; SBStyle: Integer): boolean;
     function GetWindowRelativePosition(Handle : hwnd; var Left, Top: integer): boolean; 
     function IntfSendsUTF8KeyPress: Boolean;  deprecated;
     function InvalidateFrame(aHandle : HWND; ARect : pRect; bErase : Boolean; BorderWidth: integer) : Boolean;
     function IsDesignerDC(WindowHandle: HWND; DC: HDC): Boolean;
     function IsMobilePlatform: Boolean;
     function IsCDIntfControl(AWinControl: TObject): Boolean; 
     function MoveWindowOrgEx(dc : hdc; dX,dY : Integer): boolean;
     function PromptUser(const DialogCaption, DialogMessage : String; DialogType : longint; Buttons : PLongint; ButtonCount, DefaultIndex, EscapeResult : Longint) : Longint;
     function PromptUserAtXY(const DialogCaption, DialogMessage : String; DialogType : longint; Buttons : PLongint; ButtonCount, DefaultIndex, EscapeResult : Longint; X, Y : Longint) : Longint;
     function RadialArc(DC: HDC; left, top, right, bottom, sx, sy, ex, ey : Integer): Boolean;
     function RadialChord(DC: HDC; x1, y1, x2, y2, sx, sy, ex, ey : Integer): Boolean;
     function RadialPie(DC: HDC; x1, y1, x2, y2, Angle1, Angle2: Integer): Boolean;
     function RawImage_CreateBitmaps(const ARawImage: TRawImage; out ABitmap, AMask: HBitmap; ASkipMask: Boolean = False): Boolean;
     function RawImage_DescriptionFromBitmap(ABitmap: HBITMAP; out ADesc: TRawImageDescription): Boolean;
     function RawImage_DescriptionFromDevice(ADC: HDC; out ADesc: TRawImageDescription): Boolean;
     function RawImage_FromBitmap(out ARawImage: TRawImage; ABitmap, AMask: HBITMAP; ARect: PRect = nil): Boolean;
     function RawImage_FromDevice(out ARawImage: TRawImage; ADC: HDC; const ARect: TRect): Boolean;
     function RawImage_QueryDescription(AFlags: TRawImageQueryFlags; var ADesc: TRawImageDescription): Boolean;
     function RegroupMenuItem(hndMenu: HMENU; GroupIndex: integer): Boolean;
     function ReleaseDesignerDC(hWnd: HWND; DC: HDC): Integer;
     procedure RemoveEventHandler(var AHandler: PEventHandler);
     procedure RemoveProcessEventHandler(var AHandler: PProcessEventHandler);
     procedure RemovePipeEventHandler(var AHandler: PPipeEventHandler);
     function RequestInput(const InputCaption, InputPrompt : String; MaskInput : Boolean; var Value : String) : Boolean; 
     procedure SendCachedLCLMessages;
     function SetCaretRespondToFocus(handle: HWND; ShowHideOnFocus: boolean): Boolean;
     function SetComboMinDropDownSize(Handle: HWND; MinItemsWidth, MinItemsHeight, MinItemCount: integer): boolean;
     procedure SetEventHandlerFlags(AHandler: PEventHandler; NewFlags: dword);
     procedure SetRubberBandRect(const ARubberBand: HWND; const ARect: TRect);
     function ShowSelectItemDialog(const AItems: TStrings): Boolean;
     function StretchMaskBlt(DestDC: HDC; X, Y, Width, Height: Integer; SrcDC: HDC; XSrc, YSrc, SrcWidth, SrcHeight: Integer; Mask:  HBITMAP; XMask, YMask: Integer; Rop: DWORD): Boolean ;
     function TextUTF8Out(DC: HDC; X, Y: Integer;  Str: PChar; Count: Longint): Boolean;
    
     function ExcludeClipRgn(dc: hdc; rgn : hrgn) : Integer;
     procedure NotifyUser(const DialogMessage : String; DialogType : longint);
     procedure NotifyUser(const DialogCaption, DialogMessage : String; DialogType : longint);
     procedure NotifyUserAtXY(const DialogMessage : String; DialogType : longint; X, Y : Longint);
     procedure NotifyUserAtXY(const DialogCaption, DialogMessage : String; DialogType : longint; X, Y : Longint); 
     function PromptUser(const DialogMessage : String; DialogType : longint; Buttons : Array of Longint; DefaultIndex, EscapeResult :  Longint) : Longint;
     function PromptUser(const DialogMessage : String; DialogType : longint; Buttons : PLongint; ButtonCount, DefaultIndex, EscapeResult : Longint) : Longint;
     function PromptUser(const DialogCaption, DialogMessage : String; DialogType : longint; Buttons : Array of Longint; DefaultIndex, EscapeResult : Longint) : Longint;
     function PromptUserAtXY(const DialogMessage : String; DialogType : longint; Buttons : Array of Longint; DefaultIndex, EscapeResult : Longint; X, Y : Longint) : Longint;
     function PromptUserAtXY(const DialogMessage : String; DialogType : longint; Buttons : PLongint; ButtonCount, DefaultIndex, EscapeResult : Longint; X, Y : Longint) : Longint;
     function PromptUserAtXY(const DialogCaption, DialogMessage : String; DialogType : longint; Buttons : Array of Longint; DefaultIndex, EscapeResult : Longint; X, Y : Longint) : Longint;

---

_Source: [https://wiki.freepascal.org/lclintf](https://web.archive.org/web/20250325085234/https://wiki.freepascal.org/lclintf)_
