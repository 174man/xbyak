
    C++��x86(IA-32), x64(AMD64, x86-64) JIT������֥� Xbyak version 2.24

-----------------------------------------------------------------------------
������

�����x86, x64(AMD64, x86-64)�Υޥ����̿�����������C++�Υ��饹�饤�֥��Ǥ���
�ץ����¹Ի���ưŪ�˥�����֥뤹�뤳�Ȥ���ǽ�Ǥ���

-----------------------------------------------------------------------------
����ħ

���إå��ե����륪��꡼
    xbyak.h�򥤥󥯥롼�ɤ�������Ǥ������Ѥ��뤳�Ȥ��Ǥ��ޤ���
    C++�����Ȥ�����Ĥ��Ƥ��뤿�ᡤ����������֥�����פǤ���
    32bit/64bitξ�б��Ǥ���
    �б��ˡ���˥å�:�ø�̿�����x86, MMX/MMX2/SSE/SSE2/SSE3/SSSE3/SSE4/FPU(����)

��Windows Xp(32bit, 64bit), Vista/Linux(32bit, 64bit)/Intel Mac�б�
    Windows Xp��Ǥ�VC2005 Express Ed., VC2008
    Windows Vista
    Linux (kernel 2.4.32)��Ǥ�gcc 4.3.0, CentOS 5.1��Ǥ�gcc 4.1.2
    Intel Mac
    �ʤɤ�ư���ǧ�򤷤Ƥ��ޤ���

�� gcc�Ǥ�and, or, xor�ʤɤ�黻�ҤȤ��Ʋ�ᤷ�Ƥ��ޤ����ᡤ
-fno-operator-names���ץ������ɲä��ƥ���ѥ��뤷�Ƥ���������

-----------------------------------------------------------------------------
������
xbyak.h
xbyak_bin2hex.h
xbyak_mnemonic.h
������Ʊ��Υѥ�������ƥ��󥯥롼�ɥѥ����ɲä��Ƥ���������

Linux�Ǥ�make install��/usr/local/include/xbyak�˥��ԡ�����ޤ���
-----------------------------------------------------------------------------
��ʸˡ

Xbyak::CodeGenerator ���饹��Ѿ��������Υ��饹�᥽�å����x86, x64������֥��
���Ҥ��ޤ������Υ᥽�åɤ�ƤӽФ����塤getCode()�᥽�åɤ�ƤӽФ���������
���ͤ�ʬ���Ȥ������ؿ��ݥ��󥿤��Ѵ��������Ѥ��ޤ���������֥륨�顼���㳰
�ˤ�����Τ���ޤ�(cf. main.cpp)��

������Ū��nasm��̿��ǳ�̤�Ĥ���Ф褤�Ǥ���

mov eax, ebx  --> mov(eax, ebx);
inc ecx           inc(ecx);
ret           --> ret();

�����ɥ�å���

(ptr|dword|word|byte) [base + index * (1|2|4|8) + displacement]
�Ȥ������ǻ��ꤷ�ޤ�������������ꤹ��ɬ�פ��ʤ��¤�ptr��Ȥ��Ф褤�Ǥ���
���쥯���ϥ��ݡ��Ȥ��Ƥ��ޤ���

mov eax, [ebx+ecx] --> mov (eax, ptr[ebx+ecx]);
test byte [esp], 4 --> test (byte [esp], 4);

(���) dword, word, byte�ϥ��饹�ѿ��Ǥ������äƤ��Ȥ���unsigned int��
�Ĥ���dword��typedef���ʤ��Ǥ���������

����٥�

L(ʸ����);
��������ޤ��������פ���Ȥ��Ϥ���ʸ�������ꤷ�ޤ����������Ȥ��ǽ�Ǥ�����
���Х��ɥ쥹��8�ӥåȤ˼��ޤ�ʤ�����T_NEAR��Ĥ��ʤ��ȼ¹Ի����㳰��ȯ��
���ޤ���

��hasUndefinedLabel()��ƤӽФ��ƿ��ʤ饸�����褬¸�ߤ��ʤ����Ȥ򼨤��ޤ���
�����ɤ�ľ���Ƥ���������

L("L1");
    jmp ("L1");

    jmp ("L2");
    ...
    ������̿��ξ�硥
    ...
L("L2");

    jmp ("L3", T_NEAR);
    ...
    ������̿�᤬������
    ...
L("L3");

<������>

1. MASM�饤����@@, @f, @b�򥵥ݡ���

  L("@@"); // <A>
  jmp("@b"); // jmp to <A>
  jmp("@f"); // jmp to <B>
  L("@@"); // <B>
  jmp("@b"); // jmp to <B>

2. ��٥�ζɽ경

�ԥꥪ�ɤǻϤޤ��٥��inLocalLabel(), outLocalLabel()�Ƕ��ळ�ȤǶɽ경�Ǥ��ޤ���

void func1()
{
    inLocalLabel();
    L(".lp"); // <A>
    ...
    jmp(".lp"); // jmpt to <A>
    outLocalLabel();
}

void func2()
{
    L(".lp"); // <B>
    func1();
    jmp(".lp"); // jmp to <B>
}

�嵭����ץ�Ǥ�inLocalLabel(), outLocalLabel()��̵���ȡ�
".lp"��٥�����������顼�ˤʤ�ޤ���

��Xbyak::CodeGenerator()���󥹥ȥ饯�����󥿥ե�����

@param maxSize [in] �������������祵����(�ǥե����2048byte)
@param userPtr [in] �桼���������

CodeGenerator(size_t maxSize = DEFAULT_MAX_CODE_SIZE, void *userPtr = 0);

�ǥե���ȥ����ɥ�������2048(=DEFAULT_MAX_CODE_SIZE)�Х��ȤǤ���
�������礭�ʥ����ɤ������������CodeGenerator()�Υ��󥹥ȥ饯���˻��ꤷ�Ƥ���������

class Quantize : public Xbyak::CodeGenerator {
public:
    Quantize()
        : CodeGenerator(8192)
    {
    }
    ...
};

# ưŪ�ˤ����ۤ����褤�Τ����¹�°���δ��������ݤǤ�äƤޤ���ġ�

�ޤ��桼���������򥳡����������祵�����ȶ��˻��ꤹ��ȡ�CodeGenerator��
���ꤵ�줿�����˥Х�������������ޤ���

���ݡ��ȴؿ��Ȥ��ƻ��ꤵ�줿���ɥ쥹�μ¹�°�����ѹ�����CodeArray::protect()��
Ϳ����줿�ݥ��󥿤��饢�饤���Ȥ��줿�ݥ��󥿤��������CodeArray::getAlignedAddress()
���Ѱդ��ޤ������ܺ٤�sample/test0.cpp��use memory allocated by user�򻲹ͤ�
���Ƥ���������

/**
    change exec permission of memory
    @param addr [in] buffer address
    @param size [in] buffer size
    @param canExec [in] true(enable to exec), false(disable to exec)
    @return true(success), false(failure)
*/
bool CodeArray::protect(const void *addr, size_t size, bool canExec);

/**
    get aligned memory pointer
*/
uint8 *CodeArray::getAlignedAddress(uint8 *addr, size_t alignedSize = ALIGN_SIZE);

����¾�ܺ٤ϳƼ掠��ץ�򻲾Ȥ��Ƥ���������
-----------------------------------------------------------------------------
���ޥ���

32bit�Ķ���ǥ���ѥ��뤹���XBYAK32����64bit�Ķ���ǥ���ѥ��뤹���XBYAK64��
�������ޤ��������64bit�Ķ���Ǥ�Windows�ʤ�XBYAK64_WIN��gcc��Ǥ�XBYAK64_GCC
���������ޤ���

-----------------------------------------------------------------------------
��������

test0.cpp ; ��ñ����(x86, x64)
quantize.cpp ; ��껻��JIT������֥�ˤ���̻Ҳ��ι�®��(x86)
calc.cpp ; Ϳ����줿¿�༰�򥢥���֥뤷�Ƽ¹�(x86, x64)
           boost(http://www.boost.org/)��ɬ��
bf.cpp ; JIT Brainfuck(x86, x64)

-----------------------------------------------------------------------------
�����

MMX/SSE̿��Ϥۤ����Ƽ�������Ƥ��ޤ�����3D Now!̿��䡤�������ü��
̿��ϸ������Ǥϼ�������Ƥ��ޤ���FPU��80bit��ư�����ϥ��ݡ��Ȥ��Ƥ��ޤ���

��������˾������Ф�Ϣ����������

-----------------------------------------------------------------------------
���饤����

�������줿������BSD�饤���󥹤˽����ޤ���
http://www.opensource.jp/licenses/bsd-license.html

sample/{echo,hello}.bf�� http://www.kmonos.net/alang/etc/brainfuck.php ����
���������ޤ�����

-----------------------------------------------------------------------------
������

2010/04/16 ver 2.24 change the prototype of rewrite() method
2010/04/15 ver 2.23 fix align() and xbyak_util.h for Mac
2010/02/16 ver 2.22 fix inLocalLabel()/outLocalLabel()
2009/12/09 ver 2.21 support cygwin(gcc 4.3.2)
2009/11/28 ver 2.20 FPU�ΰ���̿�᥵�ݡ���
2009/06/25 ver 2.11 64bit�⡼�ɤǤ� mov(qword[rax], imm); ����(thanks to Martin����)
2009/03/10 ver 2.10 jmp/call reg64�ξ�Ĺ��REG.W���
2009/02/24 ver 2.09 movq reg64, mmx/xmm; movq mmx/xmm, reg64�ɲ�
2009/02/13 ver 2.08 movd(xmm7, dword[eax])��0x66����Ȥ��Х�����(thanks to Gabest����)
2008/12/30 ver 2.07 call()�����Х��ɥ쥹��8bit�ʲ��ΤȤ��ΥХ�����(thanks to kato����)
2008/09/18 ver 2.06 @@, @f, @b�ȥ�٥�ζɽ경��ǽ�ɲ�(thanks to nobu-q����)
2008/09/18 ver 2.05 ptr [rip + 32bit offset]���ݡ���(thanks to �Ļҿ�(Dango-Chu)����)
2008/06/03 ver 2.04 align()�Υݥ��ߥ�������mov(ptr[eax],1);�ʤɤ򥨥顼��
2008/06/02 ver 2.03 �桼��������ꥤ�󥿥ե��������ݡ���
2008/05/26 ver 2.02 protect()(on Linux)������������ˤʤ뤳�Ȥ�����Τ���(thanks to sinichiro_h����)
2008/04/30 ver 2.01 cmpxchg16b, cdqe�ɲ�
2008/04/29 ver 2.00 x86/x64-64�Ǹ���
2008/04/25 ver 1.90 x86�Ǧ¸���
2008/04/18 ver 1.12 ����������
2008/04/14 ver 1.11 ����������
2008/03/12 ver 1.10 bsf/bsr�ɲ�(˺��Ƥ���)
2008/02/14 ver 1.09 sub eax, 1234��16bit�⡼�ɤǽ��Ϥ���Ƥ����Τ���(thanks to Robert����)
2007/11/05 ver 1.08 lock, xadd, xchg�ɲ�
2007/11/02 ver 1.07 SSSE3/SSE4�б�(thanks to �Ļҿ�(Dango-Chu)����)
2007/09/25 ver 1.06 call((int)�ؿ��ݥ���); jmp((int)�ؿ��ݥ���);�Υ��ݡ���
2007/08/04 ver 1.05 �٤�������
2007/02/04 �����ؤΥ����פ�T_NEAR��Ĥ��ʤ��Ȥ���8bit���Х��ɥ쥹������ʤ�
           �����㳰��ȯ�����ʤ��Х��ν���
2007/01/21 [disp]�η��Υ��ɥ쥹�����ΥХ�����
           mov (eax|ax|al, [disp]); mov([disp], eax|ax|al);��û��ɽ������
2007/01/17 web�ڡ�������
2007/01/04 ��������

-----------------------------------------------------------------------------
�������

��������(MITSUNARI Shigeo, herumi at nifty dot com)

---
$Revision: 1.55 $
$Date: 2010/04/16 03:48:59 $
