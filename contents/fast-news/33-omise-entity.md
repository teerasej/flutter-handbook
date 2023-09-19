
# สร้าง entity ของ JSON ที่ต้องใช้งานกับ Omise

เราจะสร้าง entity ตัวแทนของ JSON ที่จะได้กลับมาจาก Web API ของ Omise ในที่นี้จะมี `Token` และ `Charge`

## A. Token

### 1. OmiseTokenReponse

```dart
// lib/domain/entities/omise_token_response/omise_token_response.dart

import 'dart:convert';

import 'card.dart';

class OmiseTokenResponse {
  String? object;
  String? id;
  bool? livemode;
  String? location;
  bool? used;
  String? chargeStatus;
  Card? card;
  String? createdAt;

  OmiseTokenResponse({
    this.object,
    this.id,
    this.livemode,
    this.location,
    this.used,
    this.chargeStatus,
    this.card,
    this.createdAt,
  });

  factory OmiseTokenResponse.fromMap(Map<String, dynamic> data) {
    return OmiseTokenResponse(
      object: data['object'] as String?,
      id: data['id'] as String?,
      livemode: data['livemode'] as bool?,
      location: data['location'] as String?,
      used: data['used'] as bool?,
      chargeStatus: data['charge_status'] as String?,
      card: data['card'] == null
          ? null
          : Card.fromMap(data['card'] as Map<String, dynamic>),
      createdAt: data['created_at'] as String?,
    );
  }

  Map<String, dynamic> toMap() => {
        'object': object,
        'id': id,
        'livemode': livemode,
        'location': location,
        'used': used,
        'charge_status': chargeStatus,
        'card': card?.toMap(),
        'created_at': createdAt,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [OmiseTokenResponse].
  factory OmiseTokenResponse.fromJson(String data) {
    return OmiseTokenResponse.fromMap(
        json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [OmiseTokenResponse] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

### 2. Card (OmiseTokenReponse)

```dart
// lib/domain/entities/omise_token_response/card.dart

import 'dart:convert';

class Card {
  String? object;
  String? id;
  bool? livemode;
  dynamic location;
  bool? deleted;
  String? street1;
  dynamic street2;
  String? city;
  dynamic state;
  String? phoneNumber;
  String? postalCode;
  String? country;
  String? financing;
  String? bank;
  String? brand;
  String? fingerprint;
  dynamic firstDigits;
  String? lastDigits;
  String? name;
  int? expirationMonth;
  int? expirationYear;
  bool? securityCodeCheck;
  dynamic tokenizationMethod;
  String? createdAt;

  Card({
    this.object,
    this.id,
    this.livemode,
    this.location,
    this.deleted,
    this.street1,
    this.street2,
    this.city,
    this.state,
    this.phoneNumber,
    this.postalCode,
    this.country,
    this.financing,
    this.bank,
    this.brand,
    this.fingerprint,
    this.firstDigits,
    this.lastDigits,
    this.name,
    this.expirationMonth,
    this.expirationYear,
    this.securityCodeCheck,
    this.tokenizationMethod,
    this.createdAt,
  });

  factory Card.fromMap(Map<String, dynamic> data) => Card(
        object: data['object'] as String?,
        id: data['id'] as String?,
        livemode: data['livemode'] as bool?,
        location: data['location'] as dynamic,
        deleted: data['deleted'] as bool?,
        street1: data['street1'] as String?,
        street2: data['street2'] as dynamic,
        city: data['city'] as String?,
        state: data['state'] as dynamic,
        phoneNumber: data['phone_number'] as String?,
        postalCode: data['postal_code'] as String?,
        country: data['country'] as String?,
        financing: data['financing'] as String?,
        bank: data['bank'] as String?,
        brand: data['brand'] as String?,
        fingerprint: data['fingerprint'] as String?,
        firstDigits: data['first_digits'] as dynamic,
        lastDigits: data['last_digits'] as String?,
        name: data['name'] as String?,
        expirationMonth: data['expiration_month'] as int?,
        expirationYear: data['expiration_year'] as int?,
        securityCodeCheck: data['security_code_check'] as bool?,
        tokenizationMethod: data['tokenization_method'] as dynamic,
        createdAt: data['created_at'] as String?,
      );

  Map<String, dynamic> toMap() => {
        'object': object,
        'id': id,
        'livemode': livemode,
        'location': location,
        'deleted': deleted,
        'street1': street1,
        'street2': street2,
        'city': city,
        'state': state,
        'phone_number': phoneNumber,
        'postal_code': postalCode,
        'country': country,
        'financing': financing,
        'bank': bank,
        'brand': brand,
        'fingerprint': fingerprint,
        'first_digits': firstDigits,
        'last_digits': lastDigits,
        'name': name,
        'expiration_month': expirationMonth,
        'expiration_year': expirationYear,
        'security_code_check': securityCodeCheck,
        'tokenization_method': tokenizationMethod,
        'created_at': createdAt,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [Card].
  factory Card.fromJson(String data) {
    return Card.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [Card] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

## B. Charge

มี 6 ไฟล์ 

### 1. omise_charge_response

```dart
// lib/domain/entities/omise_charge_response/omise_charge_response.dart

import 'dart:convert';

import 'card.dart';
import 'metadata.dart';
import 'platform_fee.dart';
import 'refunds.dart';
import 'transaction_fees.dart';

class OmiseChargeResponse {
  String? object;
  String? id;
  String? location;
  int? amount;
  int? net;
  int? fee;
  int? feeVat;
  int? interest;
  int? interestVat;
  int? fundingAmount;
  int? refundedAmount;
  TransactionFees? transactionFees;
  PlatformFee? platformFee;
  String? currency;
  String? fundingCurrency;
  String? ip;
  Refunds? refunds;
  dynamic link;
  dynamic description;
  Metadata? metadata;
  Card? card;
  dynamic source;
  dynamic schedule;
  dynamic customer;
  dynamic dispute;
  String? transaction;
  dynamic failureCode;
  dynamic failureMessage;
  String? status;
  String? authorizeUri;
  String? returnUri;
  String? createdAt;
  String? paidAt;
  String? expiresAt;
  dynamic expiredAt;
  dynamic reversedAt;
  bool? zeroInterestInstallments;
  dynamic branch;
  dynamic terminal;
  dynamic device;
  bool? authorized;
  bool? capturable;
  bool? capture;
  bool? disputable;
  bool? livemode;
  bool? refundable;
  bool? reversed;
  bool? reversible;
  bool? voided;
  bool? paid;
  bool? expired;

  OmiseChargeResponse({
    this.object,
    this.id,
    this.location,
    this.amount,
    this.net,
    this.fee,
    this.feeVat,
    this.interest,
    this.interestVat,
    this.fundingAmount,
    this.refundedAmount,
    this.transactionFees,
    this.platformFee,
    this.currency,
    this.fundingCurrency,
    this.ip,
    this.refunds,
    this.link,
    this.description,
    this.metadata,
    this.card,
    this.source,
    this.schedule,
    this.customer,
    this.dispute,
    this.transaction,
    this.failureCode,
    this.failureMessage,
    this.status,
    this.authorizeUri,
    this.returnUri,
    this.createdAt,
    this.paidAt,
    this.expiresAt,
    this.expiredAt,
    this.reversedAt,
    this.zeroInterestInstallments,
    this.branch,
    this.terminal,
    this.device,
    this.authorized,
    this.capturable,
    this.capture,
    this.disputable,
    this.livemode,
    this.refundable,
    this.reversed,
    this.reversible,
    this.voided,
    this.paid,
    this.expired,
  });

  factory OmiseChargeResponse.fromMap(Map<String, dynamic> data) {
    return OmiseChargeResponse(
      object: data['object'] as String?,
      id: data['id'] as String?,
      location: data['location'] as String?,
      amount: data['amount'] as int?,
      net: data['net'] as int?,
      fee: data['fee'] as int?,
      feeVat: data['fee_vat'] as int?,
      interest: data['interest'] as int?,
      interestVat: data['interest_vat'] as int?,
      fundingAmount: data['funding_amount'] as int?,
      refundedAmount: data['refunded_amount'] as int?,
      transactionFees: data['transaction_fees'] == null
          ? null
          : TransactionFees.fromMap(
              data['transaction_fees'] as Map<String, dynamic>),
      platformFee: data['platform_fee'] == null
          ? null
          : PlatformFee.fromMap(data['platform_fee'] as Map<String, dynamic>),
      currency: data['currency'] as String?,
      fundingCurrency: data['funding_currency'] as String?,
      ip: data['ip'] as String?,
      refunds: data['refunds'] == null
          ? null
          : Refunds.fromMap(data['refunds'] as Map<String, dynamic>),
      link: data['link'] as dynamic,
      description: data['description'] as dynamic,
      metadata: data['metadata'] == null
          ? null
          : Metadata.fromMap(data['metadata'] as Map<String, dynamic>),
      card: data['card'] == null
          ? null
          : Card.fromMap(data['card'] as Map<String, dynamic>),
      source: data['source'] as dynamic,
      schedule: data['schedule'] as dynamic,
      customer: data['customer'] as dynamic,
      dispute: data['dispute'] as dynamic,
      transaction: data['transaction'] as String?,
      failureCode: data['failure_code'] as dynamic,
      failureMessage: data['failure_message'] as dynamic,
      status: data['status'] as String?,
      authorizeUri: data['authorize_uri'] as String?,
      returnUri: data['return_uri'] as String?,
      createdAt: data['created_at'] as String?,
      paidAt: data['paid_at'] as String?,
      expiresAt: data['expires_at'] as String?,
      expiredAt: data['expired_at'] as dynamic,
      reversedAt: data['reversed_at'] as dynamic,
      zeroInterestInstallments: data['zero_interest_installments'] as bool?,
      branch: data['branch'] as dynamic,
      terminal: data['terminal'] as dynamic,
      device: data['device'] as dynamic,
      authorized: data['authorized'] as bool?,
      capturable: data['capturable'] as bool?,
      capture: data['capture'] as bool?,
      disputable: data['disputable'] as bool?,
      livemode: data['livemode'] as bool?,
      refundable: data['refundable'] as bool?,
      reversed: data['reversed'] as bool?,
      reversible: data['reversible'] as bool?,
      voided: data['voided'] as bool?,
      paid: data['paid'] as bool?,
      expired: data['expired'] as bool?,
    );
  }

  Map<String, dynamic> toMap() => {
        'object': object,
        'id': id,
        'location': location,
        'amount': amount,
        'net': net,
        'fee': fee,
        'fee_vat': feeVat,
        'interest': interest,
        'interest_vat': interestVat,
        'funding_amount': fundingAmount,
        'refunded_amount': refundedAmount,
        'transaction_fees': transactionFees?.toMap(),
        'platform_fee': platformFee?.toMap(),
        'currency': currency,
        'funding_currency': fundingCurrency,
        'ip': ip,
        'refunds': refunds?.toMap(),
        'link': link,
        'description': description,
        'metadata': metadata?.toMap(),
        'card': card?.toMap(),
        'source': source,
        'schedule': schedule,
        'customer': customer,
        'dispute': dispute,
        'transaction': transaction,
        'failure_code': failureCode,
        'failure_message': failureMessage,
        'status': status,
        'authorize_uri': authorizeUri,
        'return_uri': returnUri,
        'created_at': createdAt,
        'paid_at': paidAt,
        'expires_at': expiresAt,
        'expired_at': expiredAt,
        'reversed_at': reversedAt,
        'zero_interest_installments': zeroInterestInstallments,
        'branch': branch,
        'terminal': terminal,
        'device': device,
        'authorized': authorized,
        'capturable': capturable,
        'capture': capture,
        'disputable': disputable,
        'livemode': livemode,
        'refundable': refundable,
        'reversed': reversed,
        'reversible': reversible,
        'voided': voided,
        'paid': paid,
        'expired': expired,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [OmiseChargeResponse].
  factory OmiseChargeResponse.fromJson(String data) {
    return OmiseChargeResponse.fromMap(
        json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [OmiseChargeResponse] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

### 2. metadata

```dart
// lib/domain/entities/omise_charge_response/metadata.dart

import 'dart:convert';

class Metadata {
  String? orderId;
  String? color;

  Metadata({this.orderId, this.color});

  factory Metadata.fromMap(Map<String, dynamic> data) => Metadata(
        orderId: data['order_id'] as String?,
        color: data['color'] as String?,
      );

  Map<String, dynamic> toMap() => {
        'order_id': orderId,
        'color': color,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [Metadata].
  factory Metadata.fromJson(String data) {
    return Metadata.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [Metadata] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

### 3. card

```dart
// lib/domain/entities/omise_charge_response/card.dart

import 'dart:convert';

class Card {
  String? object;
  String? id;
  bool? livemode;
  dynamic location;
  bool? deleted;
  String? street1;
  dynamic street2;
  String? city;
  dynamic state;
  String? phoneNumber;
  String? postalCode;
  String? country;
  String? financing;
  String? bank;
  String? brand;
  String? fingerprint;
  dynamic firstDigits;
  String? lastDigits;
  String? name;
  int? expirationMonth;
  int? expirationYear;
  bool? securityCodeCheck;
  dynamic tokenizationMethod;
  String? createdAt;

  Card({
    this.object,
    this.id,
    this.livemode,
    this.location,
    this.deleted,
    this.street1,
    this.street2,
    this.city,
    this.state,
    this.phoneNumber,
    this.postalCode,
    this.country,
    this.financing,
    this.bank,
    this.brand,
    this.fingerprint,
    this.firstDigits,
    this.lastDigits,
    this.name,
    this.expirationMonth,
    this.expirationYear,
    this.securityCodeCheck,
    this.tokenizationMethod,
    this.createdAt,
  });

  factory Card.fromMap(Map<String, dynamic> data) => Card(
        object: data['object'] as String?,
        id: data['id'] as String?,
        livemode: data['livemode'] as bool?,
        location: data['location'] as dynamic,
        deleted: data['deleted'] as bool?,
        street1: data['street1'] as String?,
        street2: data['street2'] as dynamic,
        city: data['city'] as String?,
        state: data['state'] as dynamic,
        phoneNumber: data['phone_number'] as String?,
        postalCode: data['postal_code'] as String?,
        country: data['country'] as String?,
        financing: data['financing'] as String?,
        bank: data['bank'] as String?,
        brand: data['brand'] as String?,
        fingerprint: data['fingerprint'] as String?,
        firstDigits: data['first_digits'] as dynamic,
        lastDigits: data['last_digits'] as String?,
        name: data['name'] as String?,
        expirationMonth: data['expiration_month'] as int?,
        expirationYear: data['expiration_year'] as int?,
        securityCodeCheck: data['security_code_check'] as bool?,
        tokenizationMethod: data['tokenization_method'] as dynamic,
        createdAt: data['created_at'] as String?,
      );

  Map<String, dynamic> toMap() => {
        'object': object,
        'id': id,
        'livemode': livemode,
        'location': location,
        'deleted': deleted,
        'street1': street1,
        'street2': street2,
        'city': city,
        'state': state,
        'phone_number': phoneNumber,
        'postal_code': postalCode,
        'country': country,
        'financing': financing,
        'bank': bank,
        'brand': brand,
        'fingerprint': fingerprint,
        'first_digits': firstDigits,
        'last_digits': lastDigits,
        'name': name,
        'expiration_month': expirationMonth,
        'expiration_year': expirationYear,
        'security_code_check': securityCodeCheck,
        'tokenization_method': tokenizationMethod,
        'created_at': createdAt,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [Card].
  factory Card.fromJson(String data) {
    return Card.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [Card] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

### 4. platform_fee

```dart
// lib/domain/entities/omise_charge_response/platform_fee.dart

import 'dart:convert';

class PlatformFee {
  dynamic fixed;
  dynamic amount;
  dynamic percentage;

  PlatformFee({this.fixed, this.amount, this.percentage});

  factory PlatformFee.fromMap(Map<String, dynamic> data) => PlatformFee(
        fixed: data['fixed'] as dynamic,
        amount: data['amount'] as dynamic,
        percentage: data['percentage'] as dynamic,
      );

  Map<String, dynamic> toMap() => {
        'fixed': fixed,
        'amount': amount,
        'percentage': percentage,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [PlatformFee].
  factory PlatformFee.fromJson(String data) {
    return PlatformFee.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [PlatformFee] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

### 5. refunds

```dart
// lib/domain/entities/omise_charge_response/refunds.dart

import 'dart:convert';

class Refunds {
  String? object;
  List<dynamic>? data;
  int? limit;
  int? offset;
  int? total;
  String? location;
  String? order;
  String? from;
  String? to;

  Refunds({
    this.object,
    this.data,
    this.limit,
    this.offset,
    this.total,
    this.location,
    this.order,
    this.from,
    this.to,
  });

  factory Refunds.fromMap(Map<String, dynamic> data) => Refunds(
        object: data['object'] as String?,
        data: data['data'] as List<dynamic>?,
        limit: data['limit'] as int?,
        offset: data['offset'] as int?,
        total: data['total'] as int?,
        location: data['location'] as String?,
        order: data['order'] as String?,
        from: data['from'] as String?,
        to: data['to'] as String?,
      );

  Map<String, dynamic> toMap() => {
        'object': object,
        'data': data,
        'limit': limit,
        'offset': offset,
        'total': total,
        'location': location,
        'order': order,
        'from': from,
        'to': to,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [Refunds].
  factory Refunds.fromJson(String data) {
    return Refunds.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [Refunds] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

### 6. transaction_fees

```dart
// lib/domain/entities/omise_charge_response/transaction_fees.dart

import 'dart:convert';

class TransactionFees {
  String? feeFlat;
  String? feeRate;
  String? vatRate;

  TransactionFees({this.feeFlat, this.feeRate, this.vatRate});

  factory TransactionFees.fromMap(Map<String, dynamic> data) {
    return TransactionFees(
      feeFlat: data['fee_flat'] as String?,
      feeRate: data['fee_rate'] as String?,
      vatRate: data['vat_rate'] as String?,
    );
  }

  Map<String, dynamic> toMap() => {
        'fee_flat': feeFlat,
        'fee_rate': feeRate,
        'vat_rate': vatRate,
      };

  /// `dart:convert`
  ///
  /// Parses the string and returns the resulting Json object as [TransactionFees].
  factory TransactionFees.fromJson(String data) {
    return TransactionFees.fromMap(json.decode(data) as Map<String, dynamic>);
  }

  /// `dart:convert`
  ///
  /// Converts [TransactionFees] to a JSON string.
  String toJson() => json.encode(toMap());
}

```

