package com.bankaccount.bulkupload.model;

import javax.persistence.*;
import lombok.Data;

@Data
@Entity
@Table(name = "bank_accounts")
public class BankAccount {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(unique = true, length = 8, nullable = false)
    private String accountId;

    @Column(nullable = false)
    private String accountName;
}
