unionpay
========

China Unionpay[1] Universal (5.0) interface for drupal.


Testing card
------------
<code>Card No: 6216261000000000018<p/>
ID Card No: 341126197709218366<p/>
Card Holder: 全渠道<p/>
Verify Code: Any 6-digits number</code>

Usage
-----
* Setup your merchant information at admin/config/services/unionpay.
* You may call unionpay_submit_form_submit($form, $form_state) with order information:<code>
    $form_state['values']['amount']: Order amount, in Yuan.<p/>
    $form_state['values']['orderno']: Order Number in your order processing module.<p/>
    $form_state['values']['ordertime']: Order generation time.</code>
    
  The function will produce plain HTML code which will redirect the browser into Unionpay interface website.
  So there must be an exit() in your caller.

* Your module will need to implement hook_gatewayreponse(array $callback), which will be called
  when the payment is successful and the result is given to this module. Where $callback looks like:<code>
    array(
		'unionpay' => array(
			'orderno' => $_POST['orderNumber'],
			'amount' => $_POST['settleAmount']/100,
			'transactionno' => $_POST['qid'],
			'settledate' => $_POST['settleDate'],
			'is_backend' => arg(2)=='response_back',
		),
	);</code>

  For the meaning of qid and settleDate, pelease refer to the manual of unionpay.
  Please note that result is reliable *only* if is_backend==TRUE, where the status of the order should be
  updated in your order process module. when is_backend==FALSE, which means the result should be displayed
  to the user via frontend.

* The module also provides a query function unionpay_querystatus().

[1]: https://online.unionpay.com
