
YUEX - Kripto Para REST Fonksiyonları

Bu doküman YUEX'in üçüncü parti sistemlerle iletişimini ve yönetilmesini sağlayan fonksiyonların kullanımını içerir.


Error Handling:
```json
{
	"error": "authentication failed", 
	"success": false
}
```

Index:

YUEX'in servisleri ve endpoint path'leri.

* YUEXBLC
  * /api/v1/blc/address/:coin
  * /api/v1/blc/lasttransactions
  * /api/v1/blc/sendaddress
  * /api/v1/blc/balance/limits
  * /api/v1/blc/deposit/create
  * /api/v1/blc/deposit/cancel
  * /api/v1/blc/deposit/fee
  * /api/v1/blc/deposit/control
  * /api/v1/blc/withdrawal/saveaddr
  * /api/v1/blc/withdrawal/deleteaddr
  * /api/v1/blc/withdrawal/listaddr
  * /api/v1/blc/withdrawal/control
  * /api/v1/blc/withdrawal/confirm
  * /api/v1/blc/withdrawal/create
  * /api/v1/blc/withdrawal/cancel
  * /api/v1/blc/withdrawal/fee
* YUEXLG
  * /api/v1/lg/lookup
  * /api/v1/lg/assets.jsp
  * /api/v1/lg/securityimage
  * /api/v1/lg/user/config
  * /api/v1/lg/user/user
  * /api/v1/lg/user/login
  * /api/v1/lg/user/login/auth
  * /api/v1/lg/user/register
  * /api/v1/lg/user/confirmemail
  * /api/v1/lg/user/confirminvitation
  * /api/v1/lg/user/checkemail
  * /api/v1/lg/user/checkphone
  * /api/v1/lg/user/checkinvitation
  * /api/v1/lg/user/resendsms
  * /api/v1/lg/user/forgotpassword
  * /api/v1/lg/user/suspectprocess
  * /api/v1/lg/user/resetpassword
  * /api/v1/lg/user/totpsecret
  * /api/v1/lg/user/invitations
  * /api/v1/lg/user/invitationinfo
  * /api/v1/lg/user/verification
  * /api/v1/lg/user/changedarklayout
  * /api/v1/lg/user/cities
  * /api/v1/lg/user/towns
  * /api/v1/lg/user/logout
  * /api/v1/lg/user/changetimeout
  * /api/v1/lg/user/changepassword
  * /api/v1/lg/user/confirmpassword
  * /api/v1/lg/user/changetotp
  * /api/v1/lg/user/confirmtotp
  * /api/v1/lg/user/checktotp
  * /api/v1/lg/user/changeantispam
  * /api/v1/lg/user/sendtotpchangesms
  * /api/v1/lg/user/verification
  * /api/v1/lg/user/verificationsave
  * /api/v1/lg/user/verificationremove
  * /api/v1/lg/user/confirmaccount
  * /api/v1/lg/user/confirmsms
  * /api/v1/lg/user/createinvitation
  * /api/v1/lg/user/cancelinvitation
* YUEXCRR
  * /api/v1/crr/pairs
  * /api/v1/crr/currencies
* YUEXCRT
  * /api/v1/crt/udf/config
  * /api/v1/crt/udf/time
  * /api/v1/crt/udf/symbols
  * /api/v1/crt/udf/history
* YUEXPLC
  * /api/v1/plc/order/new
  * /api/v1/plc/order/cancel
  * /api/v1/plc/order/active/all
  * /api/v1/plc/order/history/all
  * /api/v1/plc/order/active/summary
  * /api/v1/plc/order/history/summary
  * /api/v1/plc/fee/now
* YUEXSPP
  * /api/v1/ssp/announcements
* YUEXAC
  * /api/v1/ac/user/lastactions
* YUEXMC
  * /api/v1/mc/addblc
  * /api/v1/mc/balance/now
* YUEXNC
  * /api/v1/nc/user/notices
  * /api/v1/nc/user/notifications
* YUEXUAC
  * /api/v1/ac/user/lastactions


# YUEXBLC

Kullanıcıların varlıklarının tutulduğu Balance servisidir.

## GET /address/:coin

Kullanıcıya istediği COIN'de yeni bir adres verir.

Request:

curl http://localhost/api/v1/blc/address/BTC

Response:

```golang
struct {
		Success bool   `json:"success"`
		Coin    string `json:"coin"`
		Address string `json:"address"`
	}
```

## GET /api/v1/blc/lasttransactions

Yatırma çekme işlemlerinin toplu listesini göstermek için kullanılır

Request:

```golang
type LastUserPayments struct {
	Page        int    `form:"page" binding:"required"`
	RowsPerPage int    `form:"rowsperpage" binding:"required"`
	SortBy      string `form:"sortby"`
	Descending  bool   `form:"descending"`
}
```

Response:

```golang
type LastUserPaymentResponse struct {
	Status              int8   `json:"status"`
	Price               string `json:"price"`
	PriceTotal          string `json:"pricetotal"`
	Fee                 string `json:"fee"`
	LastBalance         string `json:"lastbalance"`
	CurrencyType        string `json:"currencytype"`
	ConfirmCountCurrent int64  `json:"confirmcountcurrent"`
	ConfirmCountTotal   int64  `json:"confirmcounttotal"`
	PaymentType         uint8  `json:"paymenttype"`
	RejectionReason     string `json:"rejectionreason"`
	CreatedAt           int64  `json:"createdat"`
}
```

## POST /api/v1/blc/sendaddress

Kullanıcın Coin adresini SMS veya Email ile almasını sağlar.

Request:

```golang
type SendAddrRequest struct {
	Coin     string `json:"coin" binding:"required"`
	SendWith string `json:"sendwith" binding:"required"`
}
```

Response:

```json
{
    success: true
}
```

## GET /balance/limits

Kulanıcının işlem limitlerini bildirir.

Request:

> curl http://locahost//balance/limits

Response:

```golang
type LimitsView struct {
	DailyDepositLimit        float64 `json:"ldd"`
	MonthlyDepositLimit      float64 `json:"lmd"`
	DailyWithdrawalLimit     float64 `json:"ldw"`
	MonthlyWithdrawalLimit   float64 `json:"lmw"`
	DailyDepositPercent      int     `json:"pdd"`
	MonthlyDepositPercent    int     `json:"pmd"`
	DailyWithdrawalPercent   int     `json:"pdw"`
	MonthlyWithdrawalPercent int     `json:"pmw"`
	DailyDepositCurrent      float64 `json:"cdd"`
	MonthlyDepositCurrent    float64 `json:"cmd"`
	DailyWithdrawalCurrent   float64 `json:"cdw"`
	MonthlyWithdrawalCurrent float64 `json:"cmw"`
	DailyDepositRemain       float64 `json:"rdd"`
	MontlyDepositRemain      float64 `json:"rmd"`
	DailyWithdrawalRemain    float64 `json:"rdw"`
	MontlyWithdrawalRemain   float64 `json:"rmw"`
}
```

## POST /api/v1/blc/deposit/create

Kullanıcı para yatırma işlemini gerçekleştirir.

Request:

```golang
type DepositCreateRequest struct {
	Amount   float64 `json:"amount" binding:"required,greaterthanzero"`
	Symbol   string  `json:"symbol" binding:"required"`
	Provider string  `json:"provider" binding:"required"`
	Address  string  `json:"address"`
}
```

Response:

```json
{"success": true,
			"error":        "",
			"global":       globalid,
			"redirection":  false,
			"redirecturl":  "",
			"bankiban":     bankIban,
			"bankreceiver": bankReceiver}
```

## PUT /api/v1/blc/deposit/cancel

Para yatırma işlemini iptal eder.

```golang
type DepositCancelRequest struct {
	GlobalID string `json:"global" binding:"required"`
}
```

```json
{
    "success": true
}
```

## POST /api/v1/blc/deposit/fee

Komisyon ve vergi tutarlarını hesaplar

Request

```golang
type DepositFeeRequest struct {
	Amount   float64 `json:"amount" binding:"required,greaterthanzero"`
	Symbol   string  `json:"symbol" binding:"required"`
	Provider string  `json:"provider" binding:"required"`
}
```

```json
{
	"success": true, 
	"fee": fee, 
	"tax": tax, 
	"total": total
}
```

## GET /deposit/control

Kullanıcının deposit isteği var mı diye kontrol eder.

Request:

curl http://localhost/api/v1/blc/deposit/control

Response:

```json
{
		"success":     true,
		"have":        true,
		"total":       activeDeposit.LastAmount,
		"amount":      activeDeposit.Amount,
		"tax":         activeDeposit.Tax,
		"fee":         activeDeposit.Fee,
		"provider":    activeDeposit.Provider,
		"global":      activeDeposit.GlobalID,
		"redirection": redirection,
		"redirecturl": redirectURL,
}
```

## POST /api/v1/blc/withdrawal/saveaddr

Kullanıcının para çekme adresini kaydeder.

```golang
type SaveAddrRequest struct {
	Name     string `json:"name" binding:"required,max=30"`
	Addr     string `json:"address" binding:"required"`
	Provider string `json:"provider" binding:"required,max=30"`
	Currency string `json:"currency" binding:"required,max=10"`
}
```

Response:

```json
{
		"success": true
}
```

## POST /api/v1/blc/withdrawal/deleteaddr

Kullanıcı para çekme adresini siler

Request:

```golang
type DeleteSavedAddr struct {
	Addr     string `json:"address" binding:"required"`
	Provider string `json:"provider" binding:"required,max=30"`
	Currency string `json:"currency" binding:"required,max=10"`
}
```

Response:

```json
{
		"success": true
}
```

## GET /api/v1/blc/withdrawal/listaddr

Kullanıcının para çekmede kullandığı adresleri listeler.

Request:

```golang
type GetSavedAddr struct {
	Provider string `json:"provider" binding:"required,max=30"`
	Currency string `json:"currency" binding:"required,max=10"`
}
```

Response: Array []UserSavedAddress

```golang
type UserSavedAddress struct {
	ID       uint64 `json:"-" gorm:"primary_key;AUTO_INCREMENT"`
	UserID   uint64 `json:"-" gorm:"column:user_id;not null;"`
	Currency string `json:"currency" gorm:"column:currency"`
	Provider string `json:"provider" gorm:"column:provider;not null;"`
	Name     string `json:"name" gorm:"column:name;size:40;not null;"`
	Addr     string `json:"addr" gorm:"column:addr;not null;"`
}
```

```json
{
		"addresses": []UserSavedAddress,
		"success":   true,
}
```


## GET /api/v1/blc/withdrawal/control

Kullanıcının withdraw isteğinden once atılır, aktif withdraw isteği var mi diye bakilir.


Request:

```golang
type ControlWithdraw struct {
	Symbol string `form:"symbol" binding:"required"`
}
```


Response:

```json
{
		"success":  true,
		"have":     true,
		"total":    activeWithdrawal.LastAmount,
		"amount":   activeWithdrawal.Amount,
		"tax":      activeWithdrawal.Tax,
		"fee":      activeWithdrawal.Fee,
		"provider": activeWithdrawal.Provider,
		"global":   activeWithdrawal.GlobalID,
}
```


## GET /api/v1/blc/withdrawal/confirm

Mail konfirmasyonundan sonra withdraw işlemini tamamlayan handler

```golang
type ConfirmWithdraw struct {
	Hash string `form:"hash" binding:"required"`
}
```

Response:

```json
{
		"success": true
}
```

## POST /api/v1/blc/withdrawal/create

Para çekme isteği gerçekleştirir.

```golang
type WithdrawalCreateRequest struct {
	Amount   float64 `json:"price" binding:"required,greaterthanzero"`
	Symbol   string  `json:"currencytype" binding:"required"`
	Provider string  `json:"provider" binding:"required"`
	Address  string  `json:"paymentaddress"`
}
```

```json
{
		"success": true
}
```

## PUT /api/v1/blc/withdrawal/cancel

Para çekme isteğini iptal eder.

Request:

```golang
type WithdrawalCancelRequest struct {
	GlobalID string `json:"global" binding:"required"`
}
```

Response:

```json
{
		"success": true
}
```


## POST /api/v1/blc/withdrawal/fee

Para çekme isteğindeki komisyonları hesaplar.

```golang
type WithdrawalFeeRequest struct {
	Amount   float64 `json:"amount" binding:"required,greaterthanzero"`
	Symbol   string  `json:"symbol" binding:"required"`
	Provider string  `json:"provider" binding:"required"`
}
```

Response:

```json
{
	"success": true, 
	"fee": fee, 
	"tax": tax, 
	"total": total
}
```

# YUEXCRR - Parabirimlerinin yönetildiği servis

Sistemin desteklediği para birimlerini kullanıcının balance'ı ile birlikte verir.

## GET /api/v1/crr/pairs

Request:

> curl http://localhost/api/v1/crr/pairs

Response:

> map[string]map[string]models.Pair

```golang
type Pair struct {
	ID      int     `json:"-" gorm:"primary_key;AUTO_INCREMENT"`
	Index   int8    `json:"-" gorm:"column:index"`
	Base    string  `json:"base" gorm:"column:base;size:255"`
	Second  string  `json:"second" gorm:"column:second;size:255"`
	Enabled bool    `json:"-" gorm:"column:enabled"`
	Min     float64 `json:"min" gorm:"column:min"`
	Max     float64 `json:"max" gorm:"column:max"`
	Bdigit  int32   `json:"bdigit" gorm:"column:bdigit"` //base second base digita, amount
	Sdigit  int32   `json:"sdigit" gorm:"column:sdigit"` //second digit, price
	Pdigit  int32   `json:"pdigit" gorm:"column:pdigit"` //price digit, order price
}
```

## /api/v1/crr/currencies

Parabirimlerini ve kullanıcı balance'ını getirir.

Request:

> curl http://localhost/api/v1/crr/currencies

Response:

```golang
type CurrencyResponse struct {
	Currencies []Currency                `json:"currencies"`
	Blocks     []models.UserPaymentBlock `json:"blocks"`
}
```

# YUEXCRT - Chart servisi

TRADINGVIEW vb. üzerindeki verileri hazırlar.

## GET /api/v1/crt/udf/config


Request:

> curl http://localhost//api/v1/crt/udf/config

Response:

```json
{
		"supported_resolutions":  config.Chart.SupportedResolutions,
		"supports_group_request": config.Chart.SupportsGroupRequest,
		"supports_marks":         config.Chart.SupportsMarks,
		"supports_search":        config.Chart.SupportsSearch,
		"supports_time":          config.Chart.SupportsTime,
}
````

## GET /api/v1/crt/udf/time

> curl http://localhost//api/v1/crt/udf/time

Response:

Unix Time

```text
1621510575
```

## GET /api/v1/crt/udf/symbols

Request:

> curl http://localhost/api/v1/crt/udf/symbols?symbol=BTC/TRY

Response:

```golang
type UDFSymbolsResponse struct {
	Name           string `json:"name"`
	Symbol         string `json:"symbol"`
	Description    string `json:"description"`
	ExchangeListed string `json:"exchange_listed"`
	ExchangeTraded string `json:"exchange_traded"`
	Minmov         string `json:"minmov"`
	Minmov2        string `json:"minmov2"`
	Pricescale     string `json:"pricescale"`
	HasDwm         string `json:"has_dwm"`
	Timezone       string `json:"timezone"`
	HasIntraday    string `json:"has_intraday"`
	HasNoVolume    string `json:"has_no_volume"`
	Type           string `json:"type"`
	Ticker         string `json:"ticker"`
	SessionRegular string `json:"session_regular"`
}
```

## GET /api/v1/crt/udf/history

Request:

> curl http://localhost/api/v1/crt/udf/history

Response:

```golang
type UDFChartHistory struct {
	Status        string    `json:"s"`
	Times         []int64   `json:"t"`
	ClosingPrices []float64 `json:"c"`
	OpeningPrices []float64 `json:"o"`
	HighPrices    []float64 `json:"h"`
	LowPrices     []float64 `json:"l"`
	Volumes       []float64 `json:"v"`
}
```

# YUEXPLC

Alım satım işlerinin yönetildiği servistir.

## GET /order/active/summary

Açık order özetini verir

Request:

```json
{
   "base":"",
   "second":""
}
```

Response:
```json
{
   "success":true,
   "records":[
      {
         "id":"",
         "status":0,
         "side":"",
         "amount":"",
         "price":"",
         "total":"",
         "stop":"",
         "createdat":0
      }
   ]
}
```

## GET /order/history/summary

Açık olmayan order özetini verir

Request:

```json
{
   "base":"",
   "second":""
}
```
Response:
```json
{
   "success":true,
   "records":[
      {
         "side":"",
         "amount":"",
         "price":"",
         "total":"",
         "createdat":0
      }
   ]
}
```

## GET /order/active/all

Açık orderları getirir

Request:

```json
{
   "page":0,
   "rowsPerPage":0,
   "sortBy":"",
   "descending":false
}
```

Response:

```golang
type UserOrderResponse struct {
	Global    string `json:"id"`
	Status    int8   `json:"status"`
	Base      string `json:"base"`
	Second    string `json:"second"`
	Type      string `json:"type"`
	Side      string `json:"side"`
	Amount    string `json:"amount"`
	Price     string `json:"price"`
	Stop      string `json:"stop"`
	Total     string `json:"total"`
	CreatedAt int64  `json:"createdat"`
	Easy      bool   `json:"easy"`
}
```

## GET /order/history/all

Tamamlanan orderlar listelenir

Request:

```json
{
   "page":0,
   "rowsPerPage":0,
   "sortBy":"",
   "descending":false
}
```
```golang
type UserInactiveOrderResponse struct {
	Base      string `json:"base"`
	Second    string `json:"second"`
	Type      string `json:"type"`
	Side      string `json:"side"`
	Amount    string `json:"amount"`
	Price     string `json:"price"`
	Total     string `json:"total"`
	Fee       string `json:"fee"`
	CreatedAt int64  `json:"createdat"`
}
```

## POST /order/new

Yeni order girilmesini sağlar

Request:

```golang
type OrderRequest struct {
	Base   string `json:"base" binding:"required"`                                                //BTC
	Second string `json:"second" binding:"required"`                                              //TRY
	Type   string `json:"type" binding:"required" validate:"market|limit|stop_market|stop_limit"` //market, limit, smarket, slimit
	Side   string `json:"side" binding:"required" validate:"buy|sell"`                            //BUY veya SELL
	Amount string `json:"amount" binding:"required"`                                              //Miktar 0.2  BTC
	Price  string `json:"price" binding:"required"`                                               //Limit 36.200 TL
	Stop   string `json:"stop" binding:"required"`                                                //Trigger fiyatı
	Easy   bool   `json:"easy"`                                                                   //Kolay Al-Sat için gerekli.
}
```

Response:
```json
{
   "error":"",
   "success":true,
   "id":0,
   "status":0,
   "amount":"",
   "base":"",
   "second":"",
   "price":"",
   "side":0,
   "stop":"",
   "total":""
}
```
## POST /order/cancel

Aktif orderı iptal etmek için kullanılır

Request:

```json
type CancelOrderRequest struct {
	Global string `json:"id" form:"id" binding:"required"`
}
```
Response:
```json
{
   "success":true,
   "id":""
}
```

## GET /fee/now

User'ın mevcut volume'ü üzerinden grafik için gerekli bilgileri getirir

Request:

```json

```

Response:
```json
{
	"success": true,
	"ucv":     "",  // user current volume
	"ucvp":    0,     // user current volume percent
	"cve":     "",  // current volume end
	"cfm":     0,  // current fee maker
	"cft":     0,  // current fee taker
	"nfm":     0, // next fee maker
	"nft":     0   // next fee taker
}

```

# YUEXSPP

Duyuruların yönetildiği servistir

## POST /announcements

Duyuruların listelenmesi sağlanır

Request:

```json
{
    "locale":"",
    "limit":0
}
```

Response:

```json
{
   "success":true,
   "anouncements":{
      "article":[
         {
            "url":"",
            "date":"",
            "title":"",
            "content":""
         }
      ]
   }
}
```

# YUEXUAC

## GET /user/lastactions

Son kullanıcı hareketlerini sayfa bazlı olarak çeker

Request:

```json
{
   "page":0,
   "rowsPerPage":0,
   "sortBy":"",
   "descending":false
}
```

Response:

```json
{
   "success":true,
   "totalrecords":0,
   "records":[
      {
         "id":0,
         "action":"",
         "data":"",
         "ipaddress":"",
         "useragent":"",
         "createdat":0,
         "userid":0
      }
   ]
}
```

# YUEXNC

Bildirim ve uyarı yönetimi servisidir

## GET /user/notifications

Kullanıcının tüm bildirim ayarlarını getirir

Request:

```json
```

Response:
```golang
[]map[string]interface{}
```

## PUT /user/notifications

Bildirim  ayarlarını değiştirmede kullanılır

Request:

```golang
type UpdateUserNotificationRequest struct {
	Notifications []Notification `json:"notifications" binding:"required"`
}

//Notification tekli notification yapısı
type Notification struct {
	Action   string `json:"action"`
	ViaEmail bool   `json:"via_email"`
	ViaSMS   bool   `json:"via_sms"`
}
```

Response:

```json
{
    "success": true
}
```

## GET /user/notices

Kullanıcının tüm bildirimlerini getirir

Request:

```json
```
Response:

```json
{
   "success":true,
   "broadcasts":[
      {
         "id":0,
         "content":"",
         "action":"",
         "userid":0,
         "type":0,
         "isreaded":false,
         "createdat":0,
         "expiredat":0
      }
   ],
   "notices":[
      {
         "id":0,
         "content":"",
         "action":"",
         "userid":0,
         "type":0,
         "isreaded":false,
         "createdat":0,
         "expiredat":0
      }
   ]
}
```

# YUEXLG - Kimlik Doğrulama Servisi

Kullanıcının kayıt ve kimlik doğrulamalarını gerçekleştirdiği servistir.

## GET /api/v1/lg/lookup

Kullanıcı sistemden yanıt alabiliyormu diye kontrol eder.

Request:

> curl http://localhost/api/v1/lg/lookup

Response:

```json
{
	Check  int
	Status bool
}
```

## GET /api/v1/lg/assets.jsp

Web uygulaması buraya ayakta olup olmadığını kontrol etmek için bilgi gönderir. Cookie ve Session burada tanımlanır.

Request:

> curl http://localhost/api/v1/lg/assets.jsp

Response:

```json
{
	Status bool `json:"status"`
}
```

## GET /api/v1/securityimage

Login aşamasında gelen güvenlik resmini output eden endpoint

## POST /api/v1/lg/user/config

Kullanıcı konfigurasyonu getirir.

Request:

> curl -X POST http://localhost/api/v1/lg/user/config

Response:

```text
token
```

## GET /api/v1/lg/user/user

User bilgilerini döndürür.

Request:

> curl http://localhost/api/v1/lg/user/user


Response:

```golang
struct {
		"success": true,
		"user": map[string]interface{}{
			"email":           user.Email,
			"phone":           util.MaskPhone(user.Phone, "*"),
			"darklayout":      user.DarkLayout,
			"ustate":          user.Status,
			"2fmethod":        user.TwoFactorMethod,
			"language":        user.Language,
			"defaultcurrency": user.DefaultCurrency,
			"firstname":       user.FirstName,
			"lastname":        user.LastName,
			"sesstimeout":     sesstimeout,
			"antispamkey":     user.AntiSpamKeyword,
		},
	}
```

## POST /api/v1/lg/user/login 

Kullanıcının sistemde oturum açmasını sağlayar.

Geri Dönüşler;

Gönderilen JSON model sunucu tarafında validate edilmediyse;
StatusUnprocessableEntity: 422

Gönderilen kullanıcı adı veritabanında bunulanamadıysa;
StatusNotFound: 404

Girilen şifre veritabanındaki ile eşleşmiyorsa;
StatusForbidden: 403

Kullanıcının mail validasyonu henüz yapılmadıysa;
StatusUnauthorized: 401

Kullanıcı login'e kapalı ise (henüz aktif edilmemiş kullanıcı);
StatusUnauthorized: 401

Kullanıcının 2FA ayarı açıksa ve doğrulanamadıysa;
StatusUnauthorized: 401

Kullanıcı bilgileri ile oturum başlatma sırasında problem olduğunda
StatusInternalServerError: 500

Request:

```json
{
    "email": "",
    "password": "",
    "recaptcha": ""
}
```

Response:

```json
{
    "success":  true
}
```

## POST /api/v1/lg/user/login/auth

Login işleminin 2. adımı olan endpoint

Request:

```golang
type TwoFactorAuthRequest struct {
	Passcode string `json:"passcode" binding:"required"`
}
```

Response:

```golang
struct {
		"success": true,
		"user": map[string]interface{}{
			"email":           user.Email,
			"phone":           util.MaskPhone(user.Phone, "*"),
			"darklayout":      user.DarkLayout,
			"ustate":          user.Status,
			"2fmethod":        user.TwoFactorMethod,
			"language":        user.Language,
			"defaultcurrency": user.DefaultCurrency,
			"firstname":       user.FirstName,
			"lastname":        user.LastName,
			"sesstimeout":     sesstimeout,
			"antispamkey":     user.AntiSpamKeyword,
		},
	}
```

## POST /api/v1/lg/user/register

Kullanıcıyı sisteme kaydeden endpoint.

Request:

```golang
type RegisterRequest struct {
	Email          string `json:"email" binding:"required,email"`
	Password       string `json:"password"`
	Language       string `json:"language"`
	InvitationCode string `json:"invitecode"`
	Recaptcha      string `json:"recaptcha"`
}
```

Response:

```json
{
	"success": true
}
```


## POST /api/v1/lg/user/confirmemail

 Kullanıcının üyelik sonrasında aktivasyon linkinden gelişini karşılayan endpoint

 Request:

 ```golang
type ConfirmRequest struct {
	Hash string `json:"hash" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/confirminvitation

ConfirmInvitation davetiye mailinden gelen link hash'i otomatik olarak buraya yönlendirir.

```golang
type ConfirmInvitation struct {
	Hash string `json:"hash" binding:"required"`
}
```

Response:

```json
{
	"invitationcode": invitation.InvitationCode,
	"success":        true,
}
```

## GET /api/v1/lg/user/checkemail

Kullanıcının kayıt aşamasında "email" uygunluğunu kontrol eden endpoint

Request:

> curl http://localhost/api/v1/lg/user/checkemail?email=username@domain.com

Response:

```json
{
		"available": bool,
}
```

## GET /api/v1/lg/user/checkphone

Kullanıcının kayıt aşamasında "phone" uygunluğunu kontrol eden endpoint

Request:

> curl http://localhost/api/v1/lg/user/checkemail?phone=00000000000

Response:

```json
{
		"available": bool,
}
```

## GET /api/v1/lg/user/checkinvitation

Davetiye sisteminin aktif veya pasif olduğunu bildiren endpoint.

Request:

> curl http://localhost/api/v1/lg/user/checkinvitation

Response:

```json
{
		"available": bool,
}
```

## POST /api/v1/lg/user/resendsms

Yeniden sms gönderimi sağlayan handler.

Request:

```golang
type ResendSMSRequest struct {
	Type string `json:"type" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/forgotpassword

Kullanıcının şifre sıfırlama isteği yaptığı endpoint

Request:

```golang
type ForgotPasswordRequest struct {
	Email     string `json:"email" binding:"required,email"`
	Recaptcha string `json:"recaptcha" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/suspectprocess

Eğer şifre sıfırlama isteğini kullanıcı atmadıysa şüpheli olarak bildirdiği endpoint

Request:

```golang
type ForgotPasswordHash struct {
	Hash string `json:"hash" binding:"required,max=64"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/resetpassword

Kullanıcı mailinden onayladıktan sonra şifreyi değiştirdiği endpoint, hash ve yeni şifreyi parametre olarak alır

Request:

```golang
type ResetForgotPasswordRequest struct {
	Hash     string `json:"hash" binding:"required,max=64"`
	Password string `json:"password" binding:"required,min=8,validpassword"`
}
```

Response:

```json
{
	"success": true
}
```

## GET /api/v1/lg/user/totpsecret

2FA güvenlik seviyesinde işlem yapabilmek için TOTP yöntemi ile "secret" oluşturan endpoint

Request:

> curl http://localhost/api/v1/lg/user/totpsecret

Response:

```json
 {
	"secret":  secret,
	"success": true,
}
```

## GET /api/v1/lg/user/invitations

Bütün davetiyeleri listeleyen endpoint

Request:

> curl http://localhost/api/v1/lg/user/invitations

Response:

```json
{
	"success": true,
	"records": []models.UserInvitation,
}
```

## GET /api/v1/lg/user/invitationinfo

Davetiyeler ile ilgi bilgileri gösterir

Request:

> curl http://localhost/api/v1/lg/user/invitationinfo

Response:

```json
{
		"success": bool,
		"limit":   int,
		"used":    int,
		"enabled": bool,
	}
```

## GET /api/v1/lg/user/verification

Kimlik doğrulaması için yüklenmiş olan belgeleri get eden endpoint.

Request:

> curl http://localhost/api/v1/lg/user/verification

Response:

```json
{
		"images": []map[string]interface{}{},
}
```

## PUT /api/v1/lg/user/changedarklayout

Kullanıcıdan gelen DarkLayout ayarını kaydeden endpoint.

Request:

```golang
type SaveSettingRequest struct {
	Value string `json:"value" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## PUT /api/v1/lg/user/changepassword

Kullanıcının şifresini değiştiren endpoint.

Request:

```golang
type ChangePasswordRequest struct {
	CurrentPassword string `json:"currentpassword" binding:"required"`
	NewPassword     string `json:"newpassword" binding:"required,min=8,validpassword"`
	Passcode        string `json:"passcode"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/confirmpassword

Kullanıcının şifre değiştirme isteğini mail ile onaylar

Request:

```golang
type ConfirmRequest struct {
	Hash string `json:"hash" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/changetotp

2FA güvenlik seviyesinde işlem yapma özelliğini aktif eden endpint

Request:

```golang
type TotpRequest struct {
	Password string `json:"password" binding:"required"`
	Passcode string `json:"passcode" binding:"required,len=6"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/confirmtotp

2FA güvenlik seviyesi ayarını aktif etmek için aktivasyon linkinden gelişini karşılayan endpoint (Email)

Request:

```golang
type ConfirmRequest struct {
	Hash string `json:"hash" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## PUT /api/v1/lg/user/changeantispam

```golang
type SaveAntispamKeyRequest struct {
	Value string `json:"value" binding:"max=12"`
}
```

Response:

```json
{
	"success": true
}
```

## PUT /api/v1/lg/user/sendtotpchangesms

2fa yöntemi değiştirme işlemi için sms gönderiminin yapıldığı handler

Request:

> curl http://localhost/api/v1/lg/user/sendtotpchangesms

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/verification

Kimlik doğrulaması için belge yükleme işlemini gerçekleştiren endpoint.

Request:

> curl http://localhost/api/v1/lg/user/verification

Response:

```json
{
	"success":  true,
	"imageurl": string,
}
```

## POST /api/v1/lg/user/verificationsave

Kimlik doğrulaması için yüklenmiş olan belgelerin tamamlandığını ve checking işlemine geçilmesini belirten endpoint.

Request:

> curl -X POST http://localhost/api/v1/lg/user/verificationsave

Response:

```json
{
	"success": true
}
```

## DELETE /api/v1/lg/user/verificationremove

Kimlik doğrulaması için yüklenmiş olan bir belgeyi sistemden temizleyen endpoint.

Request:

```golang
type VerificationRemoveRequest struct {
	ImageDocType string `form:"imagedoctype" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/confirmaccount

Diğer üyelik bilgilerinin alındığı endpoint.

Request:

```golang
type ConfirmAccountRequest struct {
	FirstName      string `json:"firstname" binding:"required"`
	LastName       string `json:"lastname" binding:"required"`
	IdentityNumber string `json:"identnumber" binding:"required"`
	Phone          string `json:"phonenumber" binding:"required"`
	BirthDate      string `json:"birthdate" binding:"required"`
	BirthCity      uint   `json:"city" binding:"required"`
	BirthTown      uint   `json:"town" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/createinvitation

Request:

```golang
type InvitationCreateRequest struct {
	Email string `json:"email" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

## POST /api/v1/lg/user/cancelinvitation


Request:

```golang
type InvitationCancelRequest struct {
	Id uint64 `json:"id" binding:"required"`
}
```

Response:

```json
{
	"success": true
}
```

# YUEXMC

Kullanıcıların bakiye durumunu gösteren servistir

## GET /balance/now

Kullanıcıların ilgili cüzdanlarındaki güncel bakiye durumunu gösterir

Request:

```json
```

Response:

```golang
type Balance struct {
	Assets   []Asset `json:"assets"`
	TotalTRY string  `json:"total_try"`
	TotalBTC string  `json:"total_btc"`
	Unit     string  `json:"unit"`
}

//Asset Balance'a tanımlı para birimlerini temsil eder.
type Asset struct {
	Name     string `json:"name"`
	Qty      string `json:"usable"`
	QtyTotal string `json:"total"`
	AsBTC    string `json:"asbtc"`
}
```

## GET /addblc

Test amaçlı TRY ve BTC cüzdanına bakiye yüklemek için kullanılır.

Request:


Response:
```json
{
  "status": true,
}
```
