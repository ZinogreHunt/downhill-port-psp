// Tes jalur build PSP -> PPSSPP (proyek port Downhill)
#include <pspkernel.h>
#include <pspdebug.h>
#include <pspdisplay.h>
#include <pspctrl.h>

PSP_MODULE_INFO("DHTEST", 0, 1, 0);
PSP_MAIN_THREAD_ATTR(THREAD_ATTR_USER);

static int exit_callback(int arg1, int arg2, void *common) {
    sceKernelExitGame();
    return 0;
}

static int callback_thread(SceSize args, void *argp) {
    int cbid = sceKernelCreateCallback("Exit Callback", exit_callback, NULL);
    sceKernelRegisterExitCallback(cbid);
    sceKernelSleepThreadCB();
    return 0;
}

static void setup_callbacks(void) {
    int thid = sceKernelCreateThread("update_thread", callback_thread, 0x11, 0xFA0, 0, NULL);
    if (thid >= 0) {
        sceKernelStartThread(thid, 0, NULL);
    }
}

int main(void) {
    SceCtrlData pad;
    unsigned int frame = 0;

    setup_callbacks();
    pspDebugScreenInit();
    sceCtrlSetSamplingCycle(0);
    sceCtrlSetSamplingMode(PSP_CTRL_MODE_ANALOG);

    for (;;) {
        sceCtrlReadBufferPositive(&pad, 1);
        pspDebugScreenSetXY(0, 0);
        pspDebugScreenPrintf("Downhill port - tes build\n\n");
        pspDebugScreenPrintf("frame : %u\n", frame++);
        pspDebugScreenPrintf("tombol: %08X\n", pad.Buttons);
        pspDebugScreenPrintf("analog: %3d %3d\n", pad.Lx, pad.Ly);
        sceDisplayWaitVblankStart();
    }
    return 0;
}
