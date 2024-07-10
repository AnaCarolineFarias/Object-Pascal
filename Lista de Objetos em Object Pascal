unit FormularioParaManipulacaoDeListasDeObjetos;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, Buttons, StdCtrls, XPMan;

type
  TfrmListaObjetos = class(TForm)
    gbDados: TGroupBox;
    edtNome: TEdit;
    lblNome: TLabel;
    lblRendimento: TLabel;
    edtRendimento: TEdit;
    lblImposto: TLabel;
    lblValorImposto: TLabel;
    gbOperacoes: TGroupBox;
    btnAdiciona: TButton;
    btnInsere: TButton;
    btnRemove: TButton;
    btnOrdena: TButton;
    btnPrimeiro: TButton;
    btnAnterior: TButton;
    btnProximo: TButton;
    btnUltimo: TButton;
    btnFechar: TBitBtn;
    gbPesquisarNome: TGroupBox;
    edtPesquisa: TEdit;
    xpmnfst1: TXPManifest;
    procedure btnFecharClick(Sender: TObject);
    procedure FormCreate(Sender: TObject);
    procedure btnAdicionaClick(Sender: TObject);
    procedure btnInsereClick(Sender: TObject);
    procedure btnOrdenaClick(Sender: TObject);
    procedure btnPrimeiroClick(Sender: TObject);
    procedure btnAnteriorClick(Sender: TObject);
    procedure btnProximoClick(Sender: TObject);
    procedure btnUltimoClick(Sender: TObject);
    procedure edtPesquisaChange(Sender: TObject);
    procedure edtNomeEnter(Sender: TObject);
    procedure edtRendimentoEnter(Sender: TObject);
    procedure btnRemoveClick(Sender: TObject);
    procedure FormDestroy(Sender: TObject);
    procedure edtNomeExit(Sender: TObject);
    procedure edtRendimentoExit(Sender: TObject);
    procedure edtRendimentoKeyPress(Sender: TObject; var Key: Char);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

  TContribuinte = class
  private
    // Campos e métodos privados devem ser definidos aqui
    FRendimento : Double;
    Fimposto : Double;
  public
    procedure SetRendimento (valor : Double);
    function calcula_imposto : Double;
    property Rendimento : Double read FRendimento write SetRendimento;
    property imposto : Double read calcula_imposto;
  protected
    // Campos e métodos protegidos devem ser definidos aqui.
  end;

    ERendimentoError = class(Exception);

    TPessoa_Fisica = class(TContribuinte)
    nome: string;
  end;

    TListaContribuinte = class(TList)
      procedure gbPesquisarNome(Nome : string);
      destructor Destroy ; override;
   end;

     function Compara(indice1, indice2 : Pointer):Integer;

var
  frmListaObjetos: TfrmListaObjetos;
  ListaContribuinte : TListaContribuinte;
  indiceatual : Integer;

implementation

{$R *.dfm}

{ TListaContribuinte }

function Compara(indice1, indice2: Pointer): Integer;
begin
  Result := AnsiCompareText(TPessoa_Fisica(indice1).nome,TPessoa_Fisica(indice2).nome);
end;

destructor TListaContribuinte.Destroy;
var
  i : Integer;
begin
  //for i := 0 count -1 do
begin
  TPessoa_Fisica(Items[i]).Destroy;
end;
end;

procedure TListaContribuinte.gbPesquisarNome(Nome: string);
var
  i : Integer;
begin
  First;
  indiceatual := 0;
  for i := 0 to Count-1 do
  begin
    if TPessoa_Fisica(Items[i]).nome = Nome then
    begin
      indiceatual := i;
      Exit;
    end;  
  end;  
end;  

procedure TfrmListaObjetos.btnFecharClick(Sender: TObject);
begin
  Application.Terminate;
end;

procedure TfrmListaObjetos.FormCreate(Sender: TObject);
begin
  ListaContribuinte := TListaContribuinte.Create;
end;

function TContribuinte.calcula_imposto:Double;
begin
  if (FRendimento < 900.0) then Fimposto := 0.0;
  if (FRendimento > 1800) then Fimposto := 0.25 * Rendimento -315.0;
  if (FRendimento >= 900)and(FRendimento <= 1800) then Fimposto := 0.15 * Rendimento -135.0;
  Result := Fimposto;
end;

procedure TContribuinte.SetRendimento (Valor : Double);
begin
  if valor < 0
  then
  raise ERendimentoError.Create('Rendimento não pode ser negativo')
 else
  FRendimento := valor;
end;

procedure TfrmListaObjetos.btnAdicionaClick(Sender: TObject);
var
  Contribuinte: TPessoa_Fisica;
begin
  Contribuinte := TPessoa_Fisica.Create;
  Contribuinte.nome := edtNome.Text;
  Contribuinte.Rendimento := StrToFloat(edtRendimento.Text);
  ListaContribuinte.Add(Contribuinte);
  indiceatual := ListaContribuinte.Count -1;
  lblValorImposto.Caption := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).calcula_imposto);
  btnRemove.Enabled := True;
end;

procedure TfrmListaObjetos.btnInsereClick(Sender: TObject);
var
  Contribuinte: TPessoa_Fisica;
begin
  Contribuinte := TPessoa_Fisica.Create;
  Contribuinte.nome := edtNome.Text;
  Contribuinte.Rendimento := StrToFloat(edtRendimento.Text);
  ListaContribuinte.Insert(indiceatual, Contribuinte);
  btnRemove.Enabled := True;
end;

procedure TfrmListaObjetos.btnOrdenaClick(Sender: TObject);
begin
   ListaContribuinte.Sort(Compara);
end;

procedure TfrmListaObjetos.btnPrimeiroClick(Sender: TObject);
begin
   ListaContribuinte.First;
   indiceatual := 0;
   edtNome.Text := TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).nome;
   edtRendimento.Text := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).Rendimento);
   lblValorImposto.Caption := FloatToStr(TPessoa_Fisica(ListaContribuinte[indiceatual]).calcula_imposto);
end;

procedure TfrmListaObjetos.btnAnteriorClick(Sender: TObject);
begin
   if indiceatual > 0 then indiceatual := indiceatual -1;
   edtNome.Text := TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).nome;
   edtRendimento.Text := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).Rendimento);
   lblValorImposto.Caption := FloatToStr(TPessoa_Fisica(ListaContribuinte[indiceatual]).calcula_imposto);
end;

procedure TfrmListaObjetos.btnProximoClick(Sender: TObject);
begin
   if indiceatual < ListaContribuinte.Count -1 then indiceatual := indiceatual +1;
   edtNome.Text := TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).nome;
   edtRendimento.Text := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).Rendimento);
   lblValorImposto.Caption := FloatToStr(TPessoa_Fisica(ListaContribuinte[indiceatual]).calcula_imposto);
end;

procedure TfrmListaObjetos.btnUltimoClick(Sender: TObject);
begin
   ListaContribuinte.Last;
   indiceatual := ListaContribuinte.Count -1;
   edtNome.Text := TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).nome;
   edtRendimento.Text := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).Rendimento);
   lblValorImposto.Caption := FloatToStr(TPessoa_Fisica(ListaContribuinte[indiceatual]).calcula_imposto);
end;

procedure TfrmListaObjetos.edtPesquisaChange(Sender: TObject);
begin
   ListaContribuinte.gbPesquisarNome(edtPesquisa.Text);
   edtNome.Text := TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).nome;
   edtRendimento.Text := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).Rendimento);
   lblValorImposto.Caption := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).calcula_imposto);
end;

procedure TfrmListaObjetos.edtNomeEnter(Sender: TObject);
begin
   edtNome.Clear;
   lblValorImposto.Caption := '0.0';
   edtNome.Color := clSkyBlue;
end;

procedure TfrmListaObjetos.edtRendimentoEnter(Sender: TObject);
begin
   edtRendimento.Clear;
   edtRendimento.Color := clSkyBlue;
end;

procedure TfrmListaObjetos.btnRemoveClick(Sender: TObject);
begin
   ListaContribuinte.Delete(indiceatual);
   if indiceatual > ListaContribuinte.Count -1 then indiceatual := ListaContribuinte.Count -1;
   if ListaContribuinte.Count > 0 then
   begin
     edtNome.Text := TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).nome;
     edtRendimento.Text := FloatToStr(TPessoa_Fisica(ListaContribuinte.Items[indiceatual]).Rendimento);
     //lblValorImposto.Caption := FloatToStr(ListaContribuinte.Items[indiceatual]).calcula_imposto);
   end
   else
   begin
     edtNome.Clear;
     edtRendimento.Clear;
     //lblValorImposto.Caption := '';
     btnRemove.Enabled := False;
   end;  
end;

procedure TfrmListaObjetos.FormDestroy(Sender: TObject);
begin
   ListaContribuinte.Free;
end;

procedure TfrmListaObjetos.edtNomeExit(Sender: TObject);
begin
   edtNome.Color := clwhite;
end;

procedure TfrmListaObjetos.edtRendimentoExit(Sender: TObject);
begin
   edtRendimento.Color := clWhite;
end;

procedure TfrmListaObjetos.edtRendimentoKeyPress(Sender: TObject;
  var Key: Char);
begin
    if not (Key in ['0'..'9', #8, #13]) then
  begin
    Key := #0; // Ignora a tecla se não for um número, backspace ou enter
  end;
end;

// Os dados inseridos na lista ficam apenas na memória, é perdido depois do encerramento da aplicação.

end.
