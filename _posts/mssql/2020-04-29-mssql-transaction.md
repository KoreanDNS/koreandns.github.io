---
title: [MSSQL] 트랜잭션 다양한 케이스 분석
author: Koreandns
date: 2020-04-29 00:00:00 +0800
categories: [MSSQL, transaction]
tags: [MSSQL, transaction]
---



[TEST CASE 1] exec t1 결과는?

```mssql
USE [master]
GO
/****** Object:  StoredProcedure [dbo].[t2]    Script Date: 2020-04-29 오전 12:07:14 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER procedure [dbo].[t2]
as
begin
	begin try
		begin transaction
			insert test2 values(1)
		commit transaction
	end try
	begin catch
		rollback transaction
		return 0
	end catch

	return 1
end

////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////

USE [master]
GO
/****** Object:  StoredProcedure [dbo].[t1]    Script Date: 2020-04-29 오전 12:06:39 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER procedure [dbo].[t1]
as
begin
	begin try
		begin transaction
			insert test1 values(1)
			exec t2
			insert test1 values('abcd')
		commit transaction
	end try
	begin catch
		rollback transaction
		return 0
	end catch
	return 1
end
```

t1함수 호출을 통해서 t1쪽에서 test1에 값1을 넣고 t2를 호출하고 있다.

t2에서는 test2에 값1을 넣고 커밋을 하고 반환을 했다. 그 다음 t1이 에러가 발생하여 롤백 처리를 하였다.



과연 이러면 어떤 결과가 나올까?

모두 다 롤백 처리가 된다.

------



[TEST CASE2] exec t1 결과는?

```mssql
USE [master]
GO
/****** Object:  StoredProcedure [dbo].[t2]    Script Date: 2020-04-29 오전 12:07:14 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER procedure [dbo].[t2]
as
begin
	begin try
		begin transaction
			insert test2 values(1)
			insert test1 values('abcd')
		commit transaction
	end try
	begin catch
		rollback transaction
		return 0
	end catch

	return 1
end

////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////

USE [master]
GO
/****** Object:  StoredProcedure [dbo].[t1]    Script Date: 2020-04-29 오전 12:06:39 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER procedure [dbo].[t1]
as
begin
	begin try
		begin transaction
			insert test1 values(1)
			exec t2
		commit transaction
	end try
	begin catch
		rollback transaction
		return 0
	end catch
	return 1
end
```

이번에는 TEST CASE1과 다르게 t1 함수쪽에서 에러 발생을 일으키지 않고, t2함수쪽에서 에러를 내고 있다. 이럴 경우에 과연 t1쪽도 롤백이 되는가?



t2 함수가 롤백이 되어서 리턴되면 t1함수쪽도 begin cat문으로 이동하여 롤백처리가 된다.

답은 모두 다 롤백 대상

------



[TEST CASE3] exec t1 결과는?

```mssql
USE [master]
GO
/****** Object:  StoredProcedure [dbo].[t2]    Script Date: 2020-04-29 오전 12:07:14 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER procedure [dbo].[t2]
as
begin
	begin try
		begin transaction
			insert test2 values(1)
			insert test1 values('abcd')
		commit transaction
	end try
	begin catch
		rollback transaction
		return 0
	end catch

	return 1
end

////////////////////////////////////////////////////////////////////////////
////////////////////////////////////////////////////////////////////////////

USE [master]
GO
/****** Object:  StoredProcedure [dbo].[t1]    Script Date: 2020-04-29 오전 12:06:39 ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
ALTER procedure [dbo].[t1]
as
begin
	begin try
		begin transaction
			insert test1 values(1)
		commit transaction
		exec t2
	end try
	begin catch
		rollback transaction
		return 0
	end catch
	return 1
end
```

t1함수쪽 트랜잭션쪽에서 이미 정상적인 값을 추가하고 커밋을 한 다음 t2함수를 호출 하였는데,

t2함수쪽에서 에러가 발생하여 롤백처리하고 리턴하고 있다. 과연 모두 롤백 대상일까?

답은 그렇지 않다. t1함수쪽은 롤백 대상이 아니다. 트랜잭션에 포함되어 있지 않기 때문에 따로 본 것이다.