# Creating a Backtrace with GDB\Backtrace

(gdb) bt
    #0  0x080733aa in TFORM1__BUTTON1CLICK (SENDER=0x403d8bd8, this=0x403d77d8) at unit1.pas:36
    #1  0x080ecd78 in TCONTROL__CLICK (this=0x403d8bd8) at control.inc:1768
    #2  0x081274bd in TBUTTONCONTROL__CLICK (this=0x403d8bd8) at buttoncontrol.inc:57
    #3  0x08129171 in TCUSTOMBUTTON__CLICK (this=0x403d8bd8) at buttons.inc:186
    #4  0x08129198 in TCUSTOMBUTTON__WMDEFAULTCLICKED (MESSAGE=
         {MSG = 1031, WPARAM = 136821192, LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, this=0x403d8bd8) at buttons.inc:198
    #5  0x0805bbf7 in SYSTEM_TOBJECT_$__DISPATCH$formal ()
    #6  0x080ec6e0 in TCONTROL__WNDPROC (THEMESSAGE=
         {MSG = 1031, WPARAM = 136821192, LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, this=0x403d8bd8) at control.inc:1447
    #7  0x080e4ed9 in TWINCONTROL__WNDPROC (MESSAGE=
         {MSG = 1031, WPARAM = 136821192, LPARAM = -1073747812, RESULT = 1075866658, WPARAMLO = 47560, WPARAMHI = 2087, LPARAMLO = 59548, LPARAMHI = 49151, RESULTLO = 27682, RESULTHI = 16416}, this=0x403d8bd8) at wincontrol.inc:2154
    #8  0x08161955 in DELIVERMESSAGE (TARGET=0x403d8bd8, AMESSAGE=void) at gtkproc.inc:3242
    #9  0x0818897c in GTKWSBUTTON_CLICKED (AWIDGET=0x8285380, AINFO=0x40453794) at gtkwsbuttons.pp:112
    #10 0x401d7877 in gtk_marshal_NONE__NONE () from /usr/lib/libgtk-1.2.so.0
    #11 0x40206e5a in gtk_signal_remove_emission_hook () from /usr/lib/libgtk-1.2.so.0
    #12 0x4020616a in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #13 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #14 0x40176e1c in gtk_button_clicked () from /usr/lib/libgtk-1.2.so.0
    #15 0x4017838a in gtk_button_get_relief () from /usr/lib/libgtk-1.2.so.0
    #16 0x401d7877 in gtk_marshal_NONE__NONE () from /usr/lib/libgtk-1.2.so.0
    #17 0x4020607b in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #18 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #19 0x40176d60 in gtk_button_released () from /usr/lib/libgtk-1.2.so.0
    #20 0x40177d26 in gtk_button_get_relief () from /usr/lib/libgtk-1.2.so.0
    #21 0x401d764d in gtk_marshal_BOOL__POINTER () from /usr/lib/libgtk-1.2.so.0
    #22 0x4020619f in gtk_signal_set_funcs () from /usr/lib/libgtk-1.2.so.0
    #23 0x40204234 in gtk_signal_emit () from /usr/lib/libgtk-1.2.so.0
    #24 0x40239fd9 in gtk_widget_event () from /usr/lib/libgtk-1.2.so.0
    #25 0x401d74d0 in gtk_propagate_event () from /usr/lib/libgtk-1.2.so.0
    #26 0x401d65c1 in gtk_main_do_event () from /usr/lib/libgtk-1.2.so.0
    #27 0x40065b44 in gdk_wm_protocols_filter () from /usr/lib/libgdk-1.2.so.0
    #28 0x4003de75 in g_get_current_time () from /usr/lib/libglib-1.2.so.0
    #29 0x4003e32c in g_get_current_time () from /usr/lib/libglib-1.2.so.0
    #30 0x4003e4f5 in g_main_iteration () from /usr/lib/libglib-1.2.so.0
    #31 0x401d6388 in gtk_main_iteration_do () from /usr/lib/libgtk-1.2.so.0
    #32 0x080b4d26 in TGTKWIDGETSET__WAITMESSAGE (this=0x40473014) at gtkobject.inc:1427
    #33 0x08070073 in TAPPLICATION__IDLE (this=0x4045b014) at application.inc:319
    #34 0x08070ffb in TAPPLICATION__HANDLEMESSAGE (this=0x4045b014) at application.inc:827
    #35 0x08071380 in RUNMESSAGE (parentfp=0xbffff5a4) at application.inc:933
    #36 0x080712d7 in TAPPLICATION__RUN (this=0x4045b014) at application.inc:944
    #37 0x080531c3 in main () at project1.lpr:13
    (gdb)

---

_Source: [https://wiki.freepascal.org/Creating_a_Backtrace_with_GDB%5CBacktrace](https://web.archive.org/web/20250512081418/https://wiki.freepascal.org/Creating_a_Backtrace_with_GDB%5CBacktrace)_
