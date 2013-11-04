program threedi;
uses graphABC;
var x12, x13, angularX: integer;
type 
 TObject=record
  grs,col:string; //poligons of one object
  poligon_center:real; //poligons center
 end;
 TPoint=record
  X,Z,Y:real; //points in space
  cZ:real; //center Z point
  Radius1,Angle1,Height1:real; //points in decart system
  DisplayX,DisplayY:integer; //points in monitor
 end;
 TObj=record
  objects:string;
  oX,oZ,Radius2,Angle2,Height2:real;
 end;
 
procedure MouseMove(x11, y11, mb: integer);begin;
  angularX := (round(-(x11 + x12) / 5));
  x13 := x11;
end;
 
begin

 var numP: array of point;setlength(numP,4);
 var coordinates: array of TPoint;
 var poligons: array of TObject;
 var objects:array of TObj;
 
 //we need load the "map", really? ok, lets start
  //load poligons
  var str, str1: string;
  var gr, h, step, n, k: integer;
  var f := new System.IO.StreamReader('poli.tde', System.Text.Encoding.Default);
  str := f.ReadLine;str1 := copy(str, 1, 4);val(str1, gr, h);step := 9; //reading numbers of poligons(+16 for step)
  setlength(poligons, gr); //set "poligons"-array value
  for var i := 0 to gr - 1 do begin 
   poligons[i].grs := copy(str, step, 15);
   step := step + 16;
  end;
  var k1 := 1;
  f.Close;str:=nil;str1:=nil; //close file and clear stat
  //may be, load coordinates?
  var l := new System.IO.StreamReader('cord.tde', System.Text.Encoding.Default);
  str := l.ReadLine;str1 := copy(str, 1, 4);val(str1, n, h);step := 9;//+10
  var radtext: array of string;var angtext: array of string;var heitext: array of string;
  var find2: integer;var find3: string;var ot: real;
  setlength(radtext, n);setlength(angtext, n);setlength(heitext, n);
  setlength(coordinates, n);
 for var i := 0 to n - 1 do 
  begin
    radtext[i] := copy(str, step, 10);val(radtext[i], coordinates[i].X, h);step := step + 11;
    if h = 1 then begin
      find2 := -1; 
      for var j := 1 to length(radtext[i]) do begin
        find3 := copy(radtext[i], j, 1);
        if find3 = '-' then begin delete(radtext[i], j, 1);insert('0', radtext[i], j);
        end;
      end;
    end;
    if h = 0 then find2 := 1;
    val(radtext[i], coordinates[i].X, h);coordinates[i].X := coordinates[i].X * find2;find2 := 1;  
    writeln(coordinates[i].X);
  end;step := step + 4;//readln();
  for var i := 0 to n - 1 do 
  begin
    angtext[i] := copy(str, step, 10);val(angtext[i], coordinates[i].Z, h);step := step + 11;
    if h = 1 then begin
      find2 := -1; 
      for var j := 1 to length(angtext[i]) do begin
        find3 := copy(angtext[i], j, 1);
        if find3 = '-' then begin delete(angtext[i], j, 1);insert('0', angtext[i], j);
        end;
      end;
    end;
    if h = 0 then find2 := 1;
    val(angtext[i], coordinates[i].Z, h);coordinates[i].Z := coordinates[i].Z * find2;find2 := 1;
    writeln(coordinates[i].Z);
  end;step := step + 4;//readln();
  for var i := 0 to n - 1 do 
  begin
    heitext[i] := copy(str, step, 10);val(heitext[i], coordinates[i].height1, h);step := step + 11;
    if h = 1 then begin
      find2 := -1; 
      for var j := 1 to length(heitext[i]) do begin
        find3 := copy(heitext[i], j, 1);
        if find3 = '-' then begin delete(heitext[i], j, 1); insert('0', heitext[i], j);
        end;
      end;
    end;
    if h = 0 then find2 := 1;
      val(heitext[i], coordinates[i].Height1, h);coordinates[i].Height1 := coordinates[i].Height1 * find2;find2 := 1;
      writeln(coordinates[i].Height1);
  end;//readln();
  step := step + 4;str1 := copy(str, step, 3);val(str1, k, h);step := step + 4;
  setlength(objects,k);
  for var i:=0 to k-1 do begin
    objects[i].objects:=copy(str, step, 3);step:=step+7;
  end;
  step:=step+4;
  for var i:=0 to k-1 do begin
    str1:=copy(str, step, 2);val(str1, objects[i].oX, h);step:=step+4;
    str1:=copy(str, step, 2);val(str1, objects[i].oZ, h);
  end;
  //we load all coordinates and poligons
  //i think, we need a math and mathing centres of objects
  {for var i := 0 TO k - 1 do begin
    objects[i].Radius2 := SQRT((objects[i].oZ * objects[i].oZ) + (objects[i].oX * objects[i].oX));
    OT := objects[i].oX / objects[i].Radius2;
    objects[i].Angle2 := 180 * (arctan((OT / (SQRT(1 - (OT * OT)))))) / pi#;
    if objects[i].oZ<0 THEN objects[i].Angle2:=180-objects[i].Angle2;
  end;
  //now for example for mathing coordinates
  //"k" - it's an object number
  for var i:=0 to n-1 do begin
    coordinates[i].Radius1 := SQRT(((coordinates[i].Z+objects[k-1].oZ) * (coordinates[i].Z+objects[k-1].oZ)) + ((coordinates[i].X+objects[k-1].oX) * (coordinates[i].X+objects[k-1].oX)));
    OT := (coordinates[i].X+objects[k-1].oX) / coordinates[i].Radius1;
    coordinates[i].Angle1 := 180 * (arctan((OT / (SQRT(1 - (OT * OT)))))) / pi#;
    if round(coordinates[i].Z+objects[k-1].oZ)<0 THEN coordinates[i].Angle1:=180-coordinates[i].Angle1;
  end;}
  //ok, may be lets math?
  while 0=0 do  begin
  for var i:=0 to k-1 do begin
    objects[i].Radius2 := SQRT((objects[i].oZ * objects[i].oZ) + (objects[i].oX * objects[i].oX));
    OT := objects[i].oX / objects[i].Radius2;
    objects[i].Angle2 := 180 * (arctan((OT / (SQRT(1 - (OT * OT)))))) / pi#;
    if objects[i].oZ<0 THEN objects[i].Angle2:=180-objects[i].Angle2;
    //so lets making searching poligons
    var min_el,max_el,mm_el:integer;
    var min_elt,max_elt:string;
    //objects[i].objects
    copy(min_elt, 1, 3);copy(max_elt, 4, 3);
    val(min_elt, min_el, h);val(max_elt, max_el, h);
    mm_el:=max_el-min_el;
    //main searching coordinates
    var text:string;
    var num:array[1..4] of integer;
    var zx,zy,am,xm,zm:real;
    for var j:=0 to mm_el do begin
    writeln(poligons[j].grs);
      text := copy(poligons[j].grs, 1, 3);
      val(text, num[1], h);
      text := copy(poligons[j].grs, 5, 3);
      val(text, num[2], h);
      text := copy(poligons[j].grs, 9, 3);
      val(text, num[3], h);
      text := copy(poligons[j].grs, 13, 3);
      val(text, num[4], h);
      for var o:=1 to 4 do begin        
        coordinates[num[o]-1].Radius1 := SQRT(((coordinates[num[o]-1].Z+objects[i].oZ) * (coordinates[num[o]-1].Z+objects[i].oZ)) + ((coordinates[num[o]-1].X+objects[i].oX) * (coordinates[num[o]-1].X+objects[i].oX)));
        OT := (coordinates[num[o]-1].X+objects[i].oX) / coordinates[num[o]-1].Radius1;
        coordinates[num[o]-1].Angle1 := 180 * (arctan((OT / (SQRT(1 - (OT * OT)))))) / pi#;
        if round(coordinates[num[o]-1].Z+objects[i].oZ)<0 THEN coordinates[num[o]-1].Angle1:=180-coordinates[num[o]-1].Angle1;        
        ZX := (Window.Width) / 2; ZY := Window.Height / 2;
        am := (coordinates[num[o]-1].Angle1 + AngularX) * (pi # / 180);
        xm := (coordinates[num[o]-1].Radius1 * sin(am));
        coordinates[num[o]-1].cZ := (coordinates[num[o]-1].Radius1 * cos(am));
        coordinates[num[o]-1].displayX := round(zx + ((xm * (17 / (17 + coordinates[num[o]-1].cZ * 0.528)))));
        coordinates[num[o]-1].displayY := round(zy + ((coordinates[num[o]-1].height1+objects[i].height2) * (17 / (17 + coordinates[num[o]-1].cZ * 0.528))));
      end;
        poligons[j].poligon_center := round(((coordinates[num[1]-1].cZ + coordinates[num[2]-1].cZ + coordinates[num[3]-1].cZ + coordinates[num[4]-1].cZ) / 4));
    end;
  end;
  //haha, its hard, really? :)
  //so, may be lets ranging? :)
  for var T := 2 TO gr - 2 do begin
    for var T1 := 0 TO gr - T do begin
      if poligons[T1].poligon_center < poligons[T1+1].poligon_center THEN begin
        SWAP(poligons[T1].grs, poligons[T1+1].grs);
      end;
    end;
  end;
  //may be lets drawing? :)
  var num:array[1..4] of integer;LockDrawing;Window.Clear();
  for var o := 0 to gr - 1 do begin
  //writeln(poligons[o].grs);
    if poligons[o].poligon_center>-360 then begin 
      str := copy(poligons[o].grs, 1, 3);
      val(str, num[1], h);
      str := copy(poligons[o].grs, 5, 3);
      val(str, num[2], h);
      str := copy(poligons[o].grs, 9, 3);
      val(str, num[3], h);
      str := copy(poligons[o].grs, 13, 3);
      val(str, num[4], h);
      numP[0].X := round(coordinates[num[1]-1].displayX);//writeln(' ',numP[0].X,' ',[num[1]]);
      numP[0].Y := round(coordinates[num[1]-1].displayY);//writeln(' ',numP[0].Y,' ',[num[1]]);
      numP[1].X := round(coordinates[num[2]-1].displayX);//writeln(' ',numP[1].X,' ',[num[2]]);
      numP[1].Y := round(coordinates[num[2]-1].displayY);//writeln(' ',numP[1].Y,' ',[num[2]]);
      numP[2].X := round(coordinates[num[3]-1].displayX);//writeln(' ',numP[2].X,' ',[num[3]]);
      numP[2].Y := round(coordinates[num[3]-1].displayY);//writeln(' ',numP[2].Y,' ',[num[3]]);
      numP[3].X := round(coordinates[num[4]-1].displayX);//writeln(' ',numP[3].X,' ',[num[4]]);
      numP[3].Y := round(coordinates[num[4]-1].displayY);//writeln(' ',numP[3].Y,' ',[num[4]]);
      setbrushcolor(clsilver);Polygon(numP);//readln();
    end;
    //readln();
    k1 += 1;SetWindowTitle('ThreeDi v1.0( Средн. FPS ' + Format('{0,5:f2}',k1/Milliseconds*1000)+')');
    Redraw;
    OnMouseMove := MouseMove; x12 := x13;
  end;
  end;
end.
