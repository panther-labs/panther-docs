---
description: >-
  This page provides an overview of the basics of AWS Certificate Manager (ACM)
  Certificate.
---

# ACM Certificate

## Resource Type

`AWS.ACM.Certificate`

## Resource ID Format

For ACM Certificates, the resource ID is the ARN as shown here:

\
`arn:aws:acm:us-east-1:123456789012:certificate/12345678-12ab-34cd-56ef-12345678`

## Background

The [ACM Certificate](https://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) resource represents public SSL/TLS certificates on your AWS based websites and applications.

## Fields

The following table describes the Fields you can use:

| Field                     | Type        | Description                                                                                                                                                                                                |
| ------------------------- | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CertificateAuthorityArn` | `String`    | The Amazon Resource Name to the [Private CA](https://docs.aws.amazon.com/acm-pca/latest/userguide/PcaWelcome.html)                                                                                         |
| `DomainName`              | `String`    | Fully qualified domain name (FQDN), such as www.example.com, that you want to secure with an ACM certificate.                                                                                              |
| `DomainValidationOptions` | `List`      | Validation information of each domain name that occurs as a result of the `RequestCertificate` request                                                                                                     |
| `ExtendedKeyUsages`       | `List`      | The Extended Key Usage X.509 v3 extension defines one or more purposes for which the public key can be used. This is in addition to or in place of the basic purposes specified by the Key Usage extension |
| `FailureReason`           | `String`    | The reason the certificate request failed                                                                                                                                                                  |
| `InUseBy`                 | `List`      | A list of ARNs for the Amazon Web Services resources that are using the certificate                                                                                                                        |
| `IssuedAt`                | `Timestamp` | The time at which the certificate was issued. This value exists only when the certificate type is AMAZON\_ISSUED                                                                                           |
| `Issuer`                  | `String`    | The name of the certificate authority that issued and signed the certificate                                                                                                                               |
| `KeyAlgorithm`            | `String`    | The algorithm that was used to generate the public-private key pair.                                                                                                                                       |
| `KeyUsages`               | `List`      | The Key Usage X.509 v3 extension defines the purpose of the public key contained in the certificate                                                                                                        |
| `NotAfter`                | `Timestamp` | The time after which the certificate is not valid                                                                                                                                                          |
| `NotBefore`               | `Timestamp` | The time before which the certificate is not valid                                                                                                                                                         |
| `Options`                 | `Map`       | Value that specifies whether to add the certificate to a transparency log                                                                                                                                  |
| `RenewalEligibility`      | `String`    | Specifies whether the certificate is eligible for renewal                                                                                                                                                  |
| `RenewalSummary`          | `String`    | Contains information about the status of [ACM's managed renewal](https://docs.aws.amazon.com/acm/latest/userguide/acm-renewal.html) for the certificate                                                    |
| `RevocationReason`        | `String`    | The reason the certificate was revoked                                                                                                                                                                     |
| `RevokedAt`               | `Timestamp` | The time at which the certificate was revoked                                                                                                                                                              |
| `Serial`                  | `String`    | The serial number of the certificate                                                                                                                                                                       |
| `SignatureAlgorithm`      | `String`    | The algorithm that was used to sign the certificate                                                                                                                                                        |
| `Status`                  | `String`    | The status of the certificate                                                                                                                                                                              |
| `Subject`                 | `String`    | The name of the entity that is associated with the public key contained in the certificate                                                                                                                 |
| `SubjectAlternativeNames` | `List`      | One or more domain names (subject alternative names) included in the certificate                                                                                                                           |
| `Type`                    | `String`    | The source of the certificate                                                                                                                                                                              |

## Example

```javascript
{
    "AccountId": "123456789012",
    "Arn": "arn:aws:acm:us-west-2:123456789012:certificate/aaaa-1111",
    "CertificateAuthorityArn": null,
    "DomainName": "staging.runpanther.xyz",
    "DomainValidationOptions": [
        {
            "DomainName": "example.com",
            "ResourceRecord": {
                "Name": "example.com.",
                "Type": "CNAME",
                "Value": "111.acm-validations.aws."
            },
            "ValidationDomain": "example.com",
            "ValidationEmails": null,
            "ValidationMethod": "DNS",
            "ValidationStatus": "SUCCESS"
        },
        {
            "DomainName": "*.example.com",
            "ResourceRecord": {
                "Name": "111.example.com.",
                "Type": "CNAME",
                "Value": "111.acm-validations.aws."
            },
            "ValidationDomain": "*.example.com",
            "ValidationEmails": null,
            "ValidationMethod": "DNS",
            "ValidationStatus": "SUCCESS"
        }
    ],
    "ExtendedKeyUsages": [
        {
            "Name": "TLS_WEB_CLIENT_AUTHENTICATION",
            "OID": "1.1.1.1.1.1.1.1.1"
        },
        {
            "Name": "TLS_WEB_SERVER_AUTHENTICATION",
            "OID": "2.2.2.2.2.2.2.2.2"
        }
    ],
    "FailureReason": null,
    "InUseBy": [
        "arn:aws:cloudfront::123456789012:distribution/AAAA"
    ],
    "IssuedAt": "2019-01-01T00:00:00Z",
    "Issuer": "Amazon",
    "KeyAlgorithm": "RSA-2048",
    "KeyUsages": [
        {
            "Name": "KEY_ENCIPHERMENT"
        },
        {
            "Name": "DIGITAL_SIGNATURE"
        }
    ],
    "Name": "example.com",
    "NotAfter": "2020-01-01T00:00:00Z",
    "NotBefore": "2019-01-01T00:00:00Z",
    "Options": {
        "CertificateTransparencyLoggingPreference": "ENABLED"
    },
    "Region": "us-west-2",
    "RenewalEligibility": "ELIGIBLE",
    "RenewalSummary": null,
    "ResourceId": "arn:aws:acm:us-west-2:123456789012:certificate/aaaa-1111",
    "ResourceType": "AWS.ACM.Certificate",
    "RevocationReason": null,
    "RevokedAt": null,
    "Serial": "00:00:00:00:00:00:00:00:00:00:00:00:de:ad:be:ef",
    "SignatureAlgorithm": "SHA256WITHRSA",
    "Status": "ISSUED",
    "Subject": "CN=staging.runpanther.xyz",
    "SubjectAlternativeNames": [
        "example.com",
        "*.example.com"
    ],
    "Tags": null,
    "TimeCreated": null,
    "Type": "AMAZON_ISSUED"
}
```

## References

* [ACM Concepts](https://docs.aws.amazon.com/acm/latest/userguide/acm-concepts.html)
