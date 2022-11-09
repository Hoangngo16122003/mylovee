# mylovee
<! DOCTYPE HTML PUBLIC "- // W3C // DTD HTML 4.0 Chuyển tiếp // EN" >
< HTML >
 < ĐẦU >
  < TITLE > MINH IT </ TITLE >
  < META  NAME = " Trình tạo " CONTENT = " EditPlus " >
  < META  NAME = " Tác giả " CONTENT = "" >
  < META  NAME = " Từ khóa " CONTENT = "" >
  < META  NAME = " Mô tả " CONTENT = "" >
  < link  rel = " stylesheet " href = " style.css " >
  < style >
  html ,  nội dung {
  chiều cao :  100 % ;
  đệm :  0 ;
  lề :  0 ;
  nền :  rgba ( 0 ,  0 ,  0 ,  0,851 );
}
canvas {
  vị trí : tuyệt đối;
  chiều rộng :  100 % ;
  chiều cao :  100 % ;
}
  </ style >
 </ HEAD >
 < CƠ THỂ >
  < div  class = " box " >
    < canvas  id = " pinkboard " > </ canvas >
  </ div >
< script >
var  settings  =  {
  hạt : {
    chiều dài :    10000 ,  // lượng hạt tối đa
    thời lượng :    4 ,  // thời lượng hạt tính bằng giây
    vận tốc : 80 ,  // vận tốc hạt tính bằng pixel / giây
    effect : - 1.3 ,  // chơi với cái này để có hiệu ứng đẹp
    size :       8 ,  // kích thước hạt tính bằng pixel
  } ,
} ;
/ *
* /
( function ( ) { var  b = 0 ; var  c = [ "ms" , "moz" , "webkit" , "o" ] ; for ( var  a = 0 ; a < c . length && ! window . requestAnimationFrame ; + + a ) { window . requestAnimationFrame = window [ c [ a] + "RequestAnimationFrame" ] ; cửa sổ . hủyAnimationFrame = window [ c [ a ] + "CancelAnimationFrame" ] || window [ c [ a ] + "CancelRequestAnimationFrame" ] } if ( ! window . requestAnimationFrame ) { window . requestAnimationFrame = function ( h , e ) { var d=new Date().getTime();var f=Math.max(0,16-(d-b));var g=window.setTimeout(function(){h(d+f)},f);b=d+f;return g } } if ( ! window . huỷAnimationFrame ) { cửa sổ . hủyAnimationFrame = function ( d ) { clearTimeout ( d ) } } } ( ) ) ;
/ *
* Lớp điểm
* /
var  Point  =  ( function ( )  {
  function  Point ( x ,  y )  {
    cái này . x  =  ( typeof  x  ! ==  'undefined' ) ? x : 0 ;
    cái này . y  =  ( typeof  y  ! ==  'undefined' ) ? y : 0 ;
  }
  Chỉ điểm . nguyên mẫu . clone  =  function ( )  {
    return  new  Point ( this . x ,  this . y ) ;
  } ;
  Chỉ điểm . nguyên mẫu . length  =  function ( length )  {
    if  ( typeof  length  ==  'undefined' )
      trả về  Toán học . sqrt ( this . x  *  this . x  +  this . y  *  this . y ) ;
    cái này . chuẩn hóa ( ) ;
    cái này . x  * =  chiều dài ;
    cái này . y  * =  chiều dài ;
    trả lại  cái này ;
  } ;
  Chỉ điểm . nguyên mẫu . normalize  =  function ( )  {
    var  length  =  this . chiều dài ( ) ;
    cái này . x  / =  chiều dài ;
    cái này . y  / =  chiều dài ;
    trả lại  cái này ;
  } ;
   điểm trở lại ;
} ) ( ) ;
/ *
* Lớp hạt
* /
var  Particle  =  ( function ( )  {
  function  Particle ( )  {
    cái này . position  =  new  Point ( ) ;
    cái này . vận tốc  =  new  Point ( ) ;
    cái này . gia tốc  =  new  Point ( ) ;
    cái này . tuổi  =  0 ;
  }
  Hạt . nguyên mẫu . khởi tạo  =  function ( x ,  y ,  dx ,  dy )  {
    cái này . chức vụ . x  =  x ;
    cái này . chức vụ . y  =  y ;
    cái này . vận tốc . x  =  dx ;
    cái này . vận tốc . y  =  dy ;
    cái này . gia tốc . x  =  dx  *  cài đặt . các hạt . hiệu lực ;
    cái này . gia tốc . cài đặt y  =  dy  *  . các hạt . hiệu lực ;
    cái này . tuổi  =  0 ;
  } ;
  Hạt . nguyên mẫu . update  =  function ( deltaTime )  {
    cái này . chức vụ . x  + =  này . vận tốc . x  *  deltaTime ;
    cái này . chức vụ . y  + =  cái này . vận tốc . y  *  deltaTime ;
    cái này . vận tốc . x  + =  này . gia tốc . x  *  deltaTime ;
    cái này . vận tốc . y  + =  cái này . gia tốc . y  *  deltaTime ;
    cái này . age  + =  deltaTime ;
  } ;
  Hạt . nguyên mẫu . draw  =  function ( bối cảnh ,  hình ảnh )  {
    chức năng  dễ dàng ( t )  {
      return  ( - t )  *  t  *  t  +  1 ;
    }
    var  size  =  hình ảnh . chiều rộng  *  dễ dàng ( cái này . tuổi  /  cài đặt . hạt . thời gian ) ;
    bối cảnh . globalAlpha  =  1  -  cái này . tuổi  /  cài đặt . các hạt . thời hạn ;
    bối cảnh . drawImage ( image ,  this . position . x  -  size  /  2 ,  this . position . y  -  size  /  2 ,  size ,  size ) ;
  } ;
  trả lại  hạt ;
} ) ( ) ;
/ *
* Lớp ParticlePool
* /
var  ParticlePool  =  ( function ( )  {
   hạt var ,
      firstActive  =  0 ,
      firstFree    =  0 ,
      thời lượng     =  cài đặt . các hạt . thời hạn ;
 
  function  ParticlePool ( chiều dài )  {
    // tạo và điền nhóm hạt
    hạt  =  new  Array ( chiều dài ) ;
    for  ( var  i  =  0 ;  i  <  hạt . chiều dài ;  i ++ )
      hạt [ i ]  =  new  Particle ( ) ;
  }
  ParticlePool . nguyên mẫu . add  =  function ( x ,  y ,  dx ,  dy )  {
    hạt [ firstFree ] . khởi tạo ( x ,  y ,  dx ,  dy ) ;
   
    // xử lý hàng đợi vòng tròn
    firstFree ++ ;
    if  ( firstFree    ==  hạt . chiều dài )  firstFree    =  0 ;
    if  ( firstActive  ==  firstFree        )  firstActive ++ ;
    if  ( firstActive  ==  hạt . chiều dài )  firstActive  =  0 ;
  } ;
  ParticlePool . nguyên mẫu . update  =  function ( deltaTime )  {
    var  i ;
   
    // cập nhật các hạt hoạt động
    if  ( firstActive  <  firstFree )  {
      for  ( i  =  firstActive ;  i  <  firstFree ;  i ++ )
        hạt [ i ] . cập nhật ( deltaTime ) ;
    }
    if  ( firstFree  <  firstActive )  {
      for  ( i  =  firstActive ;  i  <  hạt . chiều dài ;  i ++ )
        hạt [ i ] . cập nhật ( deltaTime ) ;
      cho  ( i  =  0 ;  i  <  firstFree ;  i ++ )
        hạt [ i ] . cập nhật ( deltaTime ) ;
    }
   
    // loại bỏ các phần tử không hoạt động
    trong khi  ( các phần tử [ firstActive ] . age  > =  time && firstActive  ! = firstFree ) {    
      firstActive ++ ;
      if  ( firstActive  ==  hạt . chiều dài )  firstActive  =  0 ;
    }
   
   
  } ;
  ParticlePool . nguyên mẫu . draw  =  function ( bối cảnh ,  hình ảnh )  {
    // vẽ các hạt hoạt động
    if  ( firstActive  <  firstFree )  {
      for  ( i  =  firstActive ;  i  <  firstFree ;  i ++ )
        hạt [ i ] . vẽ ( bối cảnh ,  hình ảnh ) ;
    }
    if  ( firstFree  <  firstActive )  {
      for  ( i  =  firstActive ;  i  <  hạt . chiều dài ;  i ++ )
        hạt [ i ] . vẽ ( bối cảnh ,  hình ảnh ) ;
      cho  ( i  =  0 ;  i  <  firstFree ;  i ++ )
        hạt [ i ] . vẽ ( bối cảnh ,  hình ảnh ) ;
    }
  } ;
  trả về  ParticlePool ;
} ) ( ) ;
/ *
* Để tất cả chúng cùng nhau
* /
( function ( canvas )  {
  var  context  =  canvas . getContext ( '2d' ) ,
      hạt  =  new  ParticlePool ( cài đặt . hạt . chiều dài ) ,
      hạtRate  =  cài đặt . các hạt . độ dài  /  cài đặt . các hạt . thời lượng ,  // hạt / giây
      thời gian ;
 
  // lấy điểm trái tim với -PI <= t <= PI
  function  pointOnHeart ( t )  {
    trả lại  Điểm mới  (
      160  *  Toán học . pow ( Toán học . sin ( t ) ,  3 ) ,
      130  *  Toán học . cos ( t )  -  50  *  Toán . cos ( 2  *  t )  -  20  *  Toán . cos ( 3  *  t )  -  10  *  Toán . cos ( 4  *  t )  +  25
    ) ;
  }
 
  // tạo hình ảnh hạt bằng canvas giả
  var  image  =  ( function ( )  {
    var  canvas   =  document . createElement ( 'canvas' ) ,
        bối cảnh  =  canvas . getContext ( '2d' ) ;
    vải bạt . width   =  cài đặt . các hạt . kích thước ;
    vải bạt . chiều cao  =  cài đặt . các hạt . kích thước ;
    // hàm helper để tạo đường dẫn
    hàm  thành ( t )  {
      var  point  =  pointOnHeart ( t ) ;
      điểm . x  =  cài đặt . các hạt . kích thước  /  2  +  điểm . x  *  cài đặt . các hạt . kích thước  /  350 ;
      điểm . y  =  cài đặt . các hạt . kích thước  /  2  -  điểm . cài đặt y  *  . các hạt . kích thước / 350 ;  
       điểm trở lại ;
    }
    // tạo đường dẫn
    bối cảnh . beginPath ( ) ;
    var  t  =  - Toán học . PI ;
    var  point  =  to ( t ) ;
    bối cảnh . moveTo ( điểm . x ,  điểm . y ) ;
    while  ( t  <  Toán học . PI )  {
      t  + =  0,01 ;  // bước chân em bé!
      point  =  to ( t ) ;
      bối cảnh . lineTo ( point . x ,  point . y ) ;
    }
    bối cảnh . closePath ( ) ;
    // tạo phần tô màu
    bối cảnh . fillStyle  =  '# f50b02' ;
    bối cảnh . điền ( ) ;
    // tạo hình ảnh
    var  image  =  new  Image ( ) ;
    hình ảnh . src  =  canvas . toDataURL ( ) ;
    trả lại  hình ảnh ;
  } ) ( ) ;
 
  // kết xuất thứ đó!
  hàm  render ( )  {
    // khung hoạt hình tiếp theo
    requestAnimationFrame ( kết xuất ) ;
   
    // cập nhật thời gian
    var  newTime    =  new  Ngày ( ) . getTime ( )  /  1000 ,
        deltaTime  =  newTime  -  ( time  ||  newTime ) ;
    time  =  newTime ;
   
    // xóa canvas
    bối cảnh . clearRect ( 0 ,  0 ,  canvas . width ,  canvas . height ) ;
   
    // tạo các hạt mới
    var  quant = grainRate  * deltaTime ;   
    for  ( var  i  =  0 ;  i  <  số tiền ;  i ++ )  {
      var  pos  =  pointOnHeart ( Math . PI  -  2  *  Math . PI  *  Math . random ( ) ) ;
      var  dir  =  pos . nhân bản ( ) . chiều dài ( cài đặt . hạt . vận tốc ) ;
      các hạt . add ( canvas . width  /  2  +  pos . x ,  canvas . height  /  2  -  pos . y ,  dir . x ,  - dir . y ) ;
    }
   
    // cập nhật và vẽ các hạt
    các hạt . cập nhật ( deltaTime ) ;
    các hạt . vẽ ( bối cảnh ,  hình ảnh ) ;
  }
 
  // xử lý (thay đổi) kích thước của canvas
  function  onResize ( )  {
    vải bạt . chiều rộng   =  canvas . clientWidth ;
    vải bạt . chiều cao  =  canvas . khách hàngHeight ;
  }
  cửa sổ . onresize  =  onResize ;
 
  // trì hoãn kết xuất bootstrap
  setTimeout ( function ( )  {
    onResize ( ) ;
    kết xuất ( ) ;
  } ,  10 ) ;
} ) ( document . getElementById ( 'pinkboard' ) ) ;
  </ script >
 </ CƠ THỂ >
</ HTML >
