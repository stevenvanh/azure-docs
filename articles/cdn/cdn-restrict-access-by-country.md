---
title: Restrict Azure CDN content by country | Microsoft Docs
description: Learn how to restrict access to your Azure CDN content using the geo-filtering feature.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''

ms.assetid: 12c17cc5-28ee-4b0b-ba22-2266be2e786a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2018
ms.author: v-deasim

---
# Restrict Azure CDN content by country

## Overview
When a user requests your content, by default, the content is served regardless of where the user made this request from. In some cases, you may want to restrict access to your content by country. This article explains how to use the *geo-filtering* feature in order to configure the service to allow or block access by country.

> [!IMPORTANT]
> The Azure CDN products all provide the same geo-filtering functionality but have a small difference in te country codes they support. For more information, see [Azure CDN Country Codes](https://msdn.microsoft.com/library/mt761717.aspx).


For information about considerations that apply to configuring this type of restriction, see [Considerations](cdn-restrict-access-by-country.md#considerations).  

![Country filtering](./media/cdn-filtering/cdn-country-filtering-akamai.png)

## Step 1: Define the directory path
> [!IMPORTANT]
> **Azure CDN Standard from Microsoft** profiles do not support path-based geo-filtering.
>


Select your endpoint within the portal, and find the Geo-Filtering tab on the left-hand navigation to find this feature.

When configuring a country filter, you must specify the relative path to the location to which users will be allowed or denied access. You can apply geo-filtering for all your files with a forward slash (/) or selected folders by specifying directory paths */pictures/*. You can also apply geo-filtering to a single file by specifying the file, and leaving out the trailing slash */pictures/city.png*.

Example directory path filter:

    /                                 
    /Photos/
    /Photos/Strasbourg/
      /Photos/Strasbourg/city.png

## Step 2: Define the action: block or allow
**Block:** Users from the specified countries will be denied access to assets requested from that recursive path. If no other country filtering options have been configured for that location, then all other users will be allowed access.

**Allow:** Only users from the specified countries will be allowed access to assets requested from that recursive path.

## Step 3: Define the countries
Select the countries that you want to block or allow for the path. 

For example, the rule of blocking /Photos/Strasbourg/ will filter files including:

    http://<endpoint>.azureedge.net/Photos/Strasbourg/1000.jpg
    http://<endpoint>.azureedge.net/Photos/Strasbourg/Cathedral/1000.jpg


### Country codes
The geo-filtering feature uses country codes to define the countries from which a request will be allowed or blocked for a secured directory. Although the Azure CDN products all provide the same geo-filtering functionality, there is a small difference in the country codes they support. For more information, see [Azure CDN Country Codes](https://msdn.microsoft.com/library/mt761717.aspx). 

## Considerations
* Changes to your country filtering configuration do not take effect immediately:
   * For **Azure CDN Standard from Microsoft** profiles, propagation usually completes in 10 minutes. 
   * For **Azure CDN Standard from Akamai** profiles, propagation usually completes within one minute. 
   * For **Azure CDN Standard from Verizon** and **Azure CDN Premium from Verizon** profiles, propagation usually completes in 10 minutes. 
 
* This feature does not support wildcard characters (for example, ‘*’).

* The geo-filtering configuration associated with the relative path will be applied recursively to that path.

* Only one rule can be applied to the same relative path. That is, you cannot create multiple country filters that point to the same relative path. However, a folder can have multiple country filters, due to the recursive nature of country filters. In other words, a subfolder of a previously configured folder can be assigned a different country filter.

