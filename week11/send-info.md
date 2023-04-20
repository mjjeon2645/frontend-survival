# 3. 배송 및 결제 정보 전달

## 강의 요약 및 노트

- 아래 코드에서 `requestPayment` 함수는 비동기로 처리되어 async 키워드가 앞에 붙었다. 리턴에 있는 request_pay 함수는 콜백함수인데 이를 비동기처럼, 즉 await가 붙은 것 처럼 사용하려면 `new Promise` 객체를 생성하여 사용하면 된다고 한다.

```typescript
export default class PaymentService {
  instance = Reflect.get(window, 'IMP');

  async requestPayment({ merchantId, product, buyer }: {
    merchantId: string;
    product: Product;
    buyer?: Buyer;
  }): Promise<{
    merchantId: string;
    transactionId: string;
  }> {
    return new Promise((resolve, reject) => {
      this.instance.request_pay({
        pg: PG_CODE,
        pay_method: 'card',
        merchant_uid: merchantId,
        name: product.name,
        amount: product.price,
        buyer_email: buyer?.email,
        buyer_name: buyer?.name,
        buyer_tel: buyer?.phoneNumber,
        buyer_addr: buyer?.address,
        buyer_postcode: buyer?.postalCode,
      }, (response: PaymentResponse) => {
        if (response.success) {
          resolve({
            merchantId: response.merchant_uid,
            transactionId: response.imp_uid ?? '',
          });
        } else {
          reject(Error(response.error_msg));
        }
      });
    });
  }
}
```

## 키워드 정리
