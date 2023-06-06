- 👋 Hi, I’m @martoonfx
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
martoonfx/martoonfx is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->

{
    "charge": "0.27819",
    "start_count": "3572",
    "status": "Partial",
    "remains": "157",
    "currency": "USD"
    "bank": "PARAM"
}

   "10": {
        "error": "Incorrect order ID"
    },
    "100": {
        "charge": "1.44219",
        "start_count": "234",
        "status": "In progress",
        "remains": "10",
        "currency": "USD"
    }
}

/* --- */

<?php
class Api
{
    /** API URL */
    public $api_url = 'https://ucuzturksmm.com/api/v2';

    /** Your API key */
    public $api_key = '';

    /** Add order */
    public function order($data)
    {
        $post = array_merge(['key' => $this->api_key, 'action' => 'add'], $data);
        return json_decode($this->connect($post));
    }

    /** Get order status  */
    public function status($order_id)
    {
        return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'action' => 'status',
                'order' => $order_id
            ])
        );
    }

    /** Get orders status */
    public function multiStatus($order_ids)
    {
        return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'action' => 'status',
                'orders' => implode(",", (array)$order_ids)
            ])
        );
    }

    /** Get services */
    public function services()
    {
        return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'action' => 'services',
            ])
        );
    }

    /** Refill order */
    public function refill(int $orderId)
    {
        return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'order' => $orderId,
            ])
        );
    }

    /** Refill orders */
    public function multiRefill(array $orderIds)
    {
        return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'orders' => implode(',', $orderIds),
            ]),
            true,
        );
    }

    /** Get refill status */
    public function refillStatus(int $refillId)
    {
         return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'refill' => $refillId,
            ])
        );
    }

    /** Get refill statuses */
    public function multiRefillStatus(array $refillIds)
    {
         return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'refills' => implode(',', $refillIds),
            ]),
            true,
        );
    }

    /** Get balance */
    public function balance()
    {
        return json_decode(
            $this->connect([
                'key' => $this->api_key,
                'action' => 'balance',
            ])
        );
    }

    private function connect($post)
    {
        $_post = [];
        if (is_array($post)) {
            foreach ($post as $name => $value) {
                $_post[] = $name . '=' . urlencode($value);
            }
        }

        $ch = curl_init($this->api_url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
        curl_setopt($ch, CURLOPT_POST, 1);
        curl_setopt($ch, CURLOPT_HEADER, 0);
        curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, 0);
        curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, 0);
        curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);

        if (is_array($post)) {
            curl_setopt($ch, CURLOPT_POSTFIELDS, join('&', $_post));
        }
        curl_setopt($ch, CURLOPT_USERAGENT, 'Mozilla/4.0 (compatible; MSIE 5.01; Windows NT 5.0)');
        $result = curl_exec($ch);
        if (curl_errno($ch) != 0 && empty($result)) {
            $result = false;
        }
        curl_close($ch);
        return $result;
    }
}

// Examples

$api = new Api();

$services = $api->services(); # Return all services

$balance = $api->balance(); # Return user balance

// Add order

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'quantity' => 100, 'runs' => 2, 'interval' => 5]); # Default

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'comments' => "good pic\ngreat photo\n:)\n;)"]); # Custom Comments

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'quantity' => 100, 'usernames' => "test, testing", 'hashtags' => "#goodphoto"]); # Mentions with Hashtags

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'usernames' => "test\nexample\nfb"]); # Mentions Custom List

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'quantity' => 100, 'hashtag' => "test"]); # Mentions Hashtag

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'quantity' => 1000, 'username' => "test"]); # Mentions User Followers

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test', 'quantity' => 1000, 'usernames' => "test"]); # Mentions

$order = $api->order(['service' => 1, 'link' => 'http://martoonlfx.com/test']); # Package

// Old posts only
$order = $api->order(['service' => 1, 'username' => 'username', 'min' => 100, 'max' => 110, 'posts' => 0, 'delay' => 30, 'expiry' => '11/11/2022']); # Subscriptions

// Unlimited new posts and 5 old posts
$order = $api->order(['service' => 1, 'username' => 'username', 'min' => 100, 'max' => 110, 'old_posts' => 5, 'delay' => 30, 'expiry' => '11/11/2022']); # Subscriptions

$order = $api->order(['service' => 1, 'link' => 'http://example.com/test', 'quantity' => 100, 'username' => "test"]); # Comment Likes

$order = $api->order(['service' => 1, 'link' => 'http://example.com/test', 'quantity' => 100, 'answer_number' => '7']); # Poll

$order = $api->order(['service' => 1, 'link' => 'http://example.com/test', 'quantity' => 100, 'groups' => "group1\ngroup2"]); # Invites from Groups


$status = $api->status($order->order); # Return status, charge, remains, start count, currency

$statuses = $api->multiStatus([1, 2, 3]); # Return orders status, charge, remains, start count, currency
$refill = (array) $api->multiRefill([1, 2]);
$refillIds = array_column($refill, 'refill');
if ($refillIds) {
    $refillStatuses = $api->multiRefillStatus($refillIds);

ID = " BEYTULLAH TANCI "
TCK = " 28447045958
PHN = " +905359394748 "
MLT = " acibademli_3482@hotmail.com " 
ADR = " Acıbadem cad. Barış Sk. No 12 Saadet Apt. D1 "
LFT - CID = " TR96 0001 0008 5271 1536 6350 01 "
ID-NAME = " BEYTULLAH TANCI " 
ID-NAME = "";

}
/* ----- */

