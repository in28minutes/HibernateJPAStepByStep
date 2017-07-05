\mvnw
```
#!/bin/sh
# ----------------------------------------------------------------------------
# Licensed to the Apache Software Foundation (ASF) under one
# or more contributor license agreements.  See the NOTICE file
# distributed with this work for additional information
# regarding copyright ownership.  The ASF licenses this file
# to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance
# with the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
# KIND, either express or implied.  See the License for the
# specific language governing permissions and limitations
# under the License.
# ----------------------------------------------------------------------------

# ----------------------------------------------------------------------------
# Maven2 Start Up Batch script
#
# Required ENV vars:
# ------------------
#   JAVA_HOME - location of a JDK home dir
#
# Optional ENV vars
# -----------------
#   M2_HOME - location of maven2's installed home dir
#   MAVEN_OPTS - parameters passed to the Java VM when running Maven
#     e.g. to debug Maven itself, use
#       set MAVEN_OPTS=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8000
#   MAVEN_SKIP_RC - flag to disable loading of mavenrc files
# ----------------------------------------------------------------------------

if [ -z "$MAVEN_SKIP_RC" ] ; then

  if [ -f /etc/mavenrc ] ; then
    . /etc/mavenrc
  fi

  if [ -f "$HOME/.mavenrc" ] ; then
    . "$HOME/.mavenrc"
  fi

fi

# OS specific support.  $var _must_ be set to either true or false.
cygwin=false;
darwin=false;
mingw=false
case "`uname`" in
  CYGWIN*) cygwin=true ;;
  MINGW*) mingw=true;;
  Darwin*) darwin=true
    # Use /usr/libexec/java_home if available, otherwise fall back to /Library/Java/Home
    # See https://developer.apple.com/library/mac/qa/qa1170/_index.html
    if [ -z "$JAVA_HOME" ]; then
      if [ -x "/usr/libexec/java_home" ]; then
        export JAVA_HOME="`/usr/libexec/java_home`"
      else
        export JAVA_HOME="/Library/Java/Home"
      fi
    fi
    ;;
esac

if [ -z "$JAVA_HOME" ] ; then
  if [ -r /etc/gentoo-release ] ; then
    JAVA_HOME=`java-config --jre-home`
  fi
fi

if [ -z "$M2_HOME" ] ; then
  ## resolve links - $0 may be a link to maven's home
  PRG="$0"

  # need this for relative symlinks
  while [ -h "$PRG" ] ; do
    ls=`ls -ld "$PRG"`
    link=`expr "$ls" : '.*-> \(.*\)$'`
    if expr "$link" : '/.*' > /dev/null; then
      PRG="$link"
    else
      PRG="`dirname "$PRG"`/$link"
    fi
  done

  saveddir=`pwd`

  M2_HOME=`dirname "$PRG"`/..

  # make it fully qualified
  M2_HOME=`cd "$M2_HOME" && pwd`

  cd "$saveddir"
  # echo Using m2 at $M2_HOME
fi

# For Cygwin, ensure paths are in UNIX format before anything is touched
if $cygwin ; then
  [ -n "$M2_HOME" ] &&
    M2_HOME=`cygpath --unix "$M2_HOME"`
  [ -n "$JAVA_HOME" ] &&
    JAVA_HOME=`cygpath --unix "$JAVA_HOME"`
  [ -n "$CLASSPATH" ] &&
    CLASSPATH=`cygpath --path --unix "$CLASSPATH"`
fi

# For Migwn, ensure paths are in UNIX format before anything is touched
if $mingw ; then
  [ -n "$M2_HOME" ] &&
    M2_HOME="`(cd "$M2_HOME"; pwd)`"
  [ -n "$JAVA_HOME" ] &&
    JAVA_HOME="`(cd "$JAVA_HOME"; pwd)`"
  # TODO classpath?
fi

if [ -z "$JAVA_HOME" ]; then
  javaExecutable="`which javac`"
  if [ -n "$javaExecutable" ] && ! [ "`expr \"$javaExecutable\" : '\([^ ]*\)'`" = "no" ]; then
    # readlink(1) is not available as standard on Solaris 10.
    readLink=`which readlink`
    if [ ! `expr "$readLink" : '\([^ ]*\)'` = "no" ]; then
      if $darwin ; then
        javaHome="`dirname \"$javaExecutable\"`"
        javaExecutable="`cd \"$javaHome\" && pwd -P`/javac"
      else
        javaExecutable="`readlink -f \"$javaExecutable\"`"
      fi
      javaHome="`dirname \"$javaExecutable\"`"
      javaHome=`expr "$javaHome" : '\(.*\)/bin'`
      JAVA_HOME="$javaHome"
      export JAVA_HOME
    fi
  fi
fi

if [ -z "$JAVACMD" ] ; then
  if [ -n "$JAVA_HOME"  ] ; then
    if [ -x "$JAVA_HOME/jre/sh/java" ] ; then
      # IBM's JDK on AIX uses strange locations for the executables
      JAVACMD="$JAVA_HOME/jre/sh/java"
    else
      JAVACMD="$JAVA_HOME/bin/java"
    fi
  else
    JAVACMD="`which java`"
  fi
fi

if [ ! -x "$JAVACMD" ] ; then
  echo "Error: JAVA_HOME is not defined correctly." >&2
  echo "  We cannot execute $JAVACMD" >&2
  exit 1
fi

if [ -z "$JAVA_HOME" ] ; then
  echo "Warning: JAVA_HOME environment variable is not set."
fi

CLASSWORLDS_LAUNCHER=org.codehaus.plexus.classworlds.launcher.Launcher

# traverses directory structure from process work directory to filesystem root
# first directory with .mvn subdirectory is considered project base directory
find_maven_basedir() {

  if [ -z "$1" ]
  then
    echo "Path not specified to find_maven_basedir"
    return 1
  fi

  basedir="$1"
  wdir="$1"
  while [ "$wdir" != '/' ] ; do
    if [ -d "$wdir"/.mvn ] ; then
      basedir=$wdir
      break
    fi
    # workaround for JBEAP-8937 (on Solaris 10/Sparc)
    if [ -d "${wdir}" ]; then
      wdir=`cd "$wdir/.."; pwd`
    fi
    # end of workaround
  done
  echo "${basedir}"
}

# concatenates all lines of a file
concat_lines() {
  if [ -f "$1" ]; then
    echo "$(tr -s '\n' ' ' < "$1")"
  fi
}

BASE_DIR=`find_maven_basedir "$(pwd)"`
if [ -z "$BASE_DIR" ]; then
  exit 1;
fi

export MAVEN_PROJECTBASEDIR=${MAVEN_BASEDIR:-"$BASE_DIR"}
echo $MAVEN_PROJECTBASEDIR
MAVEN_OPTS="$(concat_lines "$MAVEN_PROJECTBASEDIR/.mvn/jvm.config") $MAVEN_OPTS"

# For Cygwin, switch paths to Windows format before running java
if $cygwin; then
  [ -n "$M2_HOME" ] &&
    M2_HOME=`cygpath --path --windows "$M2_HOME"`
  [ -n "$JAVA_HOME" ] &&
    JAVA_HOME=`cygpath --path --windows "$JAVA_HOME"`
  [ -n "$CLASSPATH" ] &&
    CLASSPATH=`cygpath --path --windows "$CLASSPATH"`
  [ -n "$MAVEN_PROJECTBASEDIR" ] &&
    MAVEN_PROJECTBASEDIR=`cygpath --path --windows "$MAVEN_PROJECTBASEDIR"`
fi

WRAPPER_LAUNCHER=org.apache.maven.wrapper.MavenWrapperMain

exec "$JAVACMD" \
  $MAVEN_OPTS \
  -classpath "$MAVEN_PROJECTBASEDIR/.mvn/wrapper/maven-wrapper.jar" \
  "-Dmaven.home=${M2_HOME}" "-Dmaven.multiModuleProjectDirectory=${MAVEN_PROJECTBASEDIR}" \
  ${WRAPPER_LAUNCHER} $MAVEN_CONFIG "$@"
```
\mvnw.cmd
```
@REM ----------------------------------------------------------------------------
@REM Licensed to the Apache Software Foundation (ASF) under one
@REM or more contributor license agreements.  See the NOTICE file
@REM distributed with this work for additional information
@REM regarding copyright ownership.  The ASF licenses this file
@REM to you under the Apache License, Version 2.0 (the
@REM "License"); you may not use this file except in compliance
@REM with the License.  You may obtain a copy of the License at
@REM
@REM    http://www.apache.org/licenses/LICENSE-2.0
@REM
@REM Unless required by applicable law or agreed to in writing,
@REM software distributed under the License is distributed on an
@REM "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
@REM KIND, either express or implied.  See the License for the
@REM specific language governing permissions and limitations
@REM under the License.
@REM ----------------------------------------------------------------------------

@REM ----------------------------------------------------------------------------
@REM Maven2 Start Up Batch script
@REM
@REM Required ENV vars:
@REM JAVA_HOME - location of a JDK home dir
@REM
@REM Optional ENV vars
@REM M2_HOME - location of maven2's installed home dir
@REM MAVEN_BATCH_ECHO - set to 'on' to enable the echoing of the batch commands
@REM MAVEN_BATCH_PAUSE - set to 'on' to wait for a key stroke before ending
@REM MAVEN_OPTS - parameters passed to the Java VM when running Maven
@REM     e.g. to debug Maven itself, use
@REM set MAVEN_OPTS=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=8000
@REM MAVEN_SKIP_RC - flag to disable loading of mavenrc files
@REM ----------------------------------------------------------------------------

@REM Begin all REM lines with '@' in case MAVEN_BATCH_ECHO is 'on'
@echo off
@REM enable echoing my setting MAVEN_BATCH_ECHO to 'on'
@if "%MAVEN_BATCH_ECHO%" == "on"  echo %MAVEN_BATCH_ECHO%

@REM set %HOME% to equivalent of $HOME
if "%HOME%" == "" (set "HOME=%HOMEDRIVE%%HOMEPATH%")

@REM Execute a user defined script before this one
if not "%MAVEN_SKIP_RC%" == "" goto skipRcPre
@REM check for pre script, once with legacy .bat ending and once with .cmd ending
if exist "%HOME%\mavenrc_pre.bat" call "%HOME%\mavenrc_pre.bat"
if exist "%HOME%\mavenrc_pre.cmd" call "%HOME%\mavenrc_pre.cmd"
:skipRcPre

@setlocal

set ERROR_CODE=0

@REM To isolate internal variables from possible post scripts, we use another setlocal
@setlocal

@REM ==== START VALIDATION ====
if not "%JAVA_HOME%" == "" goto OkJHome

echo.
echo Error: JAVA_HOME not found in your environment. >&2
echo Please set the JAVA_HOME variable in your environment to match the >&2
echo location of your Java installation. >&2
echo.
goto error

:OkJHome
if exist "%JAVA_HOME%\bin\java.exe" goto init

echo.
echo Error: JAVA_HOME is set to an invalid directory. >&2
echo JAVA_HOME = "%JAVA_HOME%" >&2
echo Please set the JAVA_HOME variable in your environment to match the >&2
echo location of your Java installation. >&2
echo.
goto error

@REM ==== END VALIDATION ====

:init

@REM Find the project base dir, i.e. the directory that contains the folder ".mvn".
@REM Fallback to current working directory if not found.

set MAVEN_PROJECTBASEDIR=%MAVEN_BASEDIR%
IF NOT "%MAVEN_PROJECTBASEDIR%"=="" goto endDetectBaseDir

set EXEC_DIR=%CD%
set WDIR=%EXEC_DIR%
:findBaseDir
IF EXIST "%WDIR%"\.mvn goto baseDirFound
cd ..
IF "%WDIR%"=="%CD%" goto baseDirNotFound
set WDIR=%CD%
goto findBaseDir

:baseDirFound
set MAVEN_PROJECTBASEDIR=%WDIR%
cd "%EXEC_DIR%"
goto endDetectBaseDir

:baseDirNotFound
set MAVEN_PROJECTBASEDIR=%EXEC_DIR%
cd "%EXEC_DIR%"

:endDetectBaseDir

IF NOT EXIST "%MAVEN_PROJECTBASEDIR%\.mvn\jvm.config" goto endReadAdditionalConfig

@setlocal EnableExtensions EnableDelayedExpansion
for /F "usebackq delims=" %%a in ("%MAVEN_PROJECTBASEDIR%\.mvn\jvm.config") do set JVM_CONFIG_MAVEN_PROPS=!JVM_CONFIG_MAVEN_PROPS! %%a
@endlocal & set JVM_CONFIG_MAVEN_PROPS=%JVM_CONFIG_MAVEN_PROPS%

:endReadAdditionalConfig

SET MAVEN_JAVA_EXE="%JAVA_HOME%\bin\java.exe"

set WRAPPER_JAR="%MAVEN_PROJECTBASEDIR%\.mvn\wrapper\maven-wrapper.jar"
set WRAPPER_LAUNCHER=org.apache.maven.wrapper.MavenWrapperMain

%MAVEN_JAVA_EXE% %JVM_CONFIG_MAVEN_PROPS% %MAVEN_OPTS% %MAVEN_DEBUG_OPTS% -classpath %WRAPPER_JAR% "-Dmaven.multiModuleProjectDirectory=%MAVEN_PROJECTBASEDIR%" %WRAPPER_LAUNCHER% %MAVEN_CONFIG% %*
if ERRORLEVEL 1 goto error
goto end

:error
set ERROR_CODE=1

:end
@endlocal & set ERROR_CODE=%ERROR_CODE%

if not "%MAVEN_SKIP_RC%" == "" goto skipRcPost
@REM check for post script, once with legacy .bat ending and once with .cmd ending
if exist "%HOME%\mavenrc_post.bat" call "%HOME%\mavenrc_post.bat"
if exist "%HOME%\mavenrc_post.cmd" call "%HOME%\mavenrc_post.cmd"
:skipRcPost

@REM pause the script if MAVEN_BATCH_PAUSE is set to 'on'
if "%MAVEN_BATCH_PAUSE%" == "on" pause

if "%MAVEN_TERMINATE_CMD%" == "on" exit %ERROR_CODE%

exit /B %ERROR_CODE%
```
\pom.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>jdbc-demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>demo</name>
	<description>Demo project for Spring Boot</description>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.3.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>

		<!-- http://localhost:8080/h2-console -->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>1.2.1</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

	<repositories>
		<repository>
			<id>spring-snapshots</id>
			<name>Spring Snapshots</name>
			<url>https://repo.spring.io/snapshot</url>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</repository>
		<repository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestone</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
	</repositories>

	<pluginRepositories>
		<pluginRepository>
			<id>spring-snapshots</id>
			<name>Spring Snapshots</name>
			<url>https://repo.spring.io/snapshot</url>
			<snapshots>
				<enabled>true</enabled>
			</snapshots>
		</pluginRepository>
		<pluginRepository>
			<id>spring-milestones</id>
			<name>Spring Milestones</name>
			<url>https://repo.spring.io/milestone</url>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>


</project>
```
\src\main\java\com\example\demo\data\student\GenericAllPurposeRepository.java
```java
package com.example.demo.data.student;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.transaction.Transactional;

import org.springframework.stereotype.Repository;

import com.example.demo.entity.Passport;
import com.example.demo.entity.Project;
import com.example.demo.entity.Student;
import com.example.demo.entity.Task;

@Repository
@Transactional
public class GenericAllPurposeRepository {

	@PersistenceContext
	private EntityManager entityManager;

	public Passport getPassport(final long id) {
		Passport passport = entityManager
				.find(Passport.class, id);
		passport.getStudent();
		return passport;
	}

	public Task createTask(Task task) {
		return entityManager.merge(task);
	}

	public Project createProject(Project project) {
		return entityManager.merge(project);
	}

	public Student createStudent(Student student) {
		return entityManager.merge(student);
	}

	public void assignStudentToProject(long studentId, long projectId) {
		Project project = entityManager.find(Project.class,
				projectId);
		Student student = entityManager.find(Student.class,
				studentId);
		project.getStudents().add(student);
	}

}
```
\src\main\java\com\example\demo\data\student\StudentRepository.java
```java
package com.example.demo.data.student;

import java.util.List;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.persistence.Query;
import javax.transaction.Transactional;

import org.springframework.stereotype.Repository;

import com.example.demo.entity.Student;

@Repository
@Transactional
public class StudentRepository {

	@PersistenceContext
	private EntityManager entityManager;

	public Student retrieveStudent(final long id) {
		return entityManager.find(Student.class, id);
	}

	public Student createStudent(Student student) {
		if (student.getPassport() != null)
			entityManager.merge(student.getPassport());
		return entityManager.merge(student);
	}

	public Student updateStudent(Student student) {
		entityManager.merge(student);
		return student;
	}

	public Student retrieveStudentsFrom(String string) {
		return null;
	}

	public List<Student> retrieveAllStudents() {
		Query query = entityManager
				.createNamedQuery("find all students");
		return query.getResultList();
	}
}
```
\src\main\java\com\example\demo\data\todo\TodoDataService.java
```java
package com.example.demo.data.todo;

import java.sql.SQLException;
import java.util.Date;
import java.util.List;

import com.example.demo.entity.Todo;

public interface TodoDataService {

	public List<Todo> retrieveTodos(String user)
			throws SQLException;

	public int addTodo(String user, String desc,
			Date targetDate, boolean isDone)
					throws SQLException;

	public Todo retrieveTodo(int id) throws SQLException;

	public void updateTodo(Todo todo) throws SQLException;

	public void deleteTodo(int id) throws SQLException;
}
```
\src\main\java\com\example\demo\data\todo\TodoJdbcService.java
```java
package com.example.demo.data.todo;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.Timestamp;
import java.util.ArrayList;
import java.util.Date;
import java.util.List;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import com.example.demo.entity.Todo;

@Component
public class TodoJdbcService implements TodoDataService {

	@Autowired
	DataSource datasource;

	// Think about exception handling
	// We are explicitly getting the connection! What if there is an
	// exception while executing the query!

	@Override
	public List<Todo> retrieveTodos(String user)
			throws SQLException {
		Connection connection = datasource.getConnection();

		PreparedStatement st = connection.prepareStatement(
				"SELECT * FROM TODO where user=?");

		st.setString(1, user);

		ResultSet resultSet = st.executeQuery();
		List<Todo> todos = new ArrayList<>();

		while (resultSet.next()) {

			Todo todo = new Todo(resultSet.getInt("id"),
					resultSet.getString("user"),
					resultSet.getString("desc"),
					resultSet.getTimestamp("target_date"),
					resultSet.getBoolean("is_done"));
			todos.add(todo);
		}

		st.close();

		connection.close();

		return todos;

	}

	@Override
	public int addTodo(String user, String desc,
			Date targetDate, boolean isDone)
					throws SQLException {
		Connection connection = datasource.getConnection();

		PreparedStatement st = connection.prepareStatement(
				"INSERT INTO todo(user, desc, target_date, is_done) VALUES (?,?,?,?)",
				Statement.RETURN_GENERATED_KEYS);

		st.setString(1, user);
		st.setString(2, desc);
		st.setTimestamp(3,
				new Timestamp(targetDate.getTime()));
		st.setBoolean(4, isDone);

		int id = st.executeUpdate();

		st.close();

		connection.close();

		return id;

	}

	@Override
	public Todo retrieveTodo(int id) throws SQLException {
		Connection connection = datasource.getConnection();

		PreparedStatement st = connection.prepareStatement(
				"SELECT * FROM TODO where id=?");

		st.setInt(1, id);

		ResultSet resultSet = st.executeQuery();

		Todo todo = null;

		if (resultSet.next()) {

			todo = new Todo(resultSet.getInt("id"),
					resultSet.getString("user"),
					resultSet.getString("desc"),
					resultSet.getTimestamp("target_date"),
					resultSet.getBoolean("is_done"));

		}

		st.close();

		connection.close();

		return todo;

	}

	@Override
	public void updateTodo(Todo todo) throws SQLException {
		Connection connection = datasource.getConnection();

		PreparedStatement st = connection.prepareStatement(
				"Update todo set user=?, desc=?, target_date=?, is_done=? where id=?");

		st.setString(1, todo.getUser());
		st.setString(2, todo.getDesc());
		st.setTimestamp(3, new Timestamp(
				todo.getTargetDate().getTime()));
		st.setBoolean(4, todo.isDone());
		st.setInt(5, todo.getId());

		st.execute();

		st.close();

		connection.close();

	}

	@Override
	public void deleteTodo(int id) throws SQLException {
		Connection connection = datasource.getConnection();

		PreparedStatement st = connection.prepareStatement(
				"delete from todo where id=?");

		st.setInt(1, id);

		st.execute();

		st.close();

		connection.close();

	}

}
```
\src\main\java\com\example\demo\data\todo\TodoJPAService.java
```java
package com.example.demo.data.todo;

import java.util.Date;
import java.util.List;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.persistence.Query;
import javax.transaction.Transactional;

import org.springframework.stereotype.Repository;

import com.example.demo.entity.Todo;

@Repository
@Transactional
public class TodoJPAService implements TodoDataService {

	@PersistenceContext
	private EntityManager entityManager;

	@Override
	public List<Todo> retrieveTodos(String user) {
		Query query = entityManager.createNamedQuery(
				"find all todos for user", Todo.class);
		query.setParameter(1, user);
		return query.getResultList();

	}

	@Override
	public int addTodo(String user, String desc,
			Date targetDate, boolean isDone) {
		Todo todo = entityManager.merge(
				new Todo(user, desc, targetDate, isDone));
		return todo.getId();
	}

	@Override
	public Todo retrieveTodo(int id) {
		return entityManager.find(Todo.class, id);
	}

	@Override
	public void updateTodo(Todo todo) {
		entityManager.merge(todo);
	}

	@Override
	public void deleteTodo(int id) {
		Todo todo = retrieveTodo(id);
		entityManager.remove(todo);
	}
}
```
\src\main\java\com\example\demo\data\todo\TodoMybatisService.java
```java
package com.example.demo.data.todo;

import java.sql.SQLException;
import java.util.Date;
import java.util.List;

import org.apache.ibatis.annotations.Delete;
import org.apache.ibatis.annotations.Insert;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.annotations.Select;
import org.apache.ibatis.annotations.Update;

import com.example.demo.entity.Todo;

@Mapper
public interface TodoMybatisService
		extends TodoDataService {

	@Override
	@Insert("INSERT INTO TODO(user, desc, target_date, is_done) VALUES (#{user}, #{desc}, #{targetDate}, #{isDone})")
	public int addTodo(@Param("user") String user,
			@Param("desc") String desc,
			@Param("targetDate") Date targetDate,
			@Param("isDone") boolean isDone)
					throws SQLException;

	@Override
	@Select("SELECT * FROM TODO WHERE id = #{id}")
	public Todo retrieveTodo(int id) throws SQLException;

	@Override
	@Update("Update todo set user=#{user}, desc=#{desc}, target_date=#{targetDate}, is_done=#{isDone} where id=#{id}")
	public void updateTodo(Todo todo) throws SQLException;

	@Override
	@Delete("DELETE FROM TODO WHERE id = #{id}")
	public void deleteTodo(int id);

	@Override
	@Select("SELECT * FROM TODO WHERE user = #{user}")
	List<Todo> retrieveTodos(@Param("user") String user);

}
```
\src\main\java\com\example\demo\data\todo\TodoSpringJdbcService.java
```java
package com.example.demo.data.todo;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Timestamp;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;
import org.springframework.stereotype.Component;

import com.example.demo.entity.Todo;

@Component
public class TodoSpringJdbcService
		implements TodoDataService {

	@Autowired
	JdbcTemplate jdbcTemplate;

	SimpleJdbcInsert insertTodo;

	@Autowired
	public void setDataSource(DataSource dataSource) {
		this.insertTodo = new SimpleJdbcInsert(dataSource)
				.withTableName("TODO")
				.usingGeneratedKeyColumns("id");
	}

	// new BeanPropertyRowMapper(TodoMapper.class)
	class TodoMapper implements RowMapper<Todo> {
		@Override
		public Todo mapRow(ResultSet rs, int rowNum)
				throws SQLException {
			Todo todo = new Todo();

			todo.setId(rs.getInt("id"));
			todo.setUser(rs.getString("user"));
			todo.setDesc(rs.getString("desc"));
			todo.setTargetDate(
					rs.getTimestamp("target_date"));
			todo.setDone(rs.getBoolean("is_done"));
			return todo;
		}
	}

	@Override
	public List<Todo> retrieveTodos(String user) {
		return jdbcTemplate.query(
				"SELECT * FROM TODO where user = ?",
				new Object[] { user }, new TodoMapper());

	}

	@Override
	public int addTodo(String user, String desc,
			Date targetDate, boolean isDone)
					throws SQLException {
		Map<String, Object> params = new HashMap<String, Object>();
		params.put("user", user);
		params.put("desc", desc);
		params.put("target_date", targetDate);
		params.put("is_done", isDone);
		Number id = insertTodo.executeAndReturnKey(params);

		return id.intValue();
	}

	@Override
	public Todo retrieveTodo(int id) {

		return jdbcTemplate.queryForObject(
				"SELECT * FROM TODO where id=?",
				new Object[] { id }, new TodoMapper());

	}

	@Override
	public void updateTodo(Todo todo) {
		jdbcTemplate
				.update("Update todo set user=?, desc=?, target_date=?, is_done=? where id=?",
						todo.getUser(), todo.getDesc(),
						new Timestamp(todo.getTargetDate()
								.getTime()),
						todo.isDone(), todo.getId());

	}

	@Override
	public void deleteTodo(int id) {
		jdbcTemplate.update("delete from todo where id=?",
				id);
	}

}
```
\src\main\java\com\example\demo\DemoApplication.java
```java
package com.example.demo;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import com.example.demo.data.student.GenericAllPurposeRepository;
import com.example.demo.data.student.StudentRepository;
import com.example.demo.data.todo.TodoDataService;
import com.example.demo.entity.Passport;
import com.example.demo.entity.Project;
import com.example.demo.entity.Student;
import com.example.demo.entity.Task;

//In almost every relationship, independent of source and target sides, one of the two sides will have the join column in its table. That side is called the owning side or the owner of the relationship. The side that does not have the join column is called the non-owning or inverse side.
//Although we have described the owning side as being determined by the data schema, the object model must indicate the owning side through the use of the relationship mapping annotations. The absence of the mappedBy element in the mapping annotation implies ownership of the relationship, while the presence of the mappedBy element means the entity is on the inverse side of the relationship. The mappedBy element is described in subsequent sections.

@SpringBootApplication
public class DemoApplication implements CommandLineRunner {

	private static final Logger log = LoggerFactory
			.getLogger(DemoApplication.class);

	@Autowired
	TodoDataService todoJPAService;

	@Autowired
	StudentRepository studentRepository;

	@Autowired
	GenericAllPurposeRepository genericRepository;

	public static void main(String[] args) {
		SpringApplication.run(DemoApplication.class, args);
	}

	@Override
	public void run(String... strings) throws Exception {

		runAllStudentExamples();

		/*
		 * int todo1 = todoJPAService.addTodo("Ranga", "Dummy10", new Date(),
		 * false);
		 * 
		 * int todo2 = todoJPAService.addTodo("Ranga", "Dummy11", new Date(),
		 * false);
		 * 
		 * log.info( "Querying for todo records where user = 'Ranga':");
		 * 
		 * todoJPAService.retrieveTodos("Ranga") .forEach(todo ->
		 * log.info(todo.toString()));
		 * 
		 * todoJPAService.updateTodo(new Todo(todo1, "Ranga", "Dummy++", new
		 * Date(), false));
		 * 
		 * log.info("Querying Todo by id " + todo1);
		 * 
		 * log.info(todoJPAService.retrieveTodo(todo1) .toString());
		 * 
		 * log.info("Deleting todo id " + todo2);
		 * 
		 * todoJPAService.deleteTodo(todo2);
		 * 
		 * log.info( "Querying for todo records where user = 'Ranga':");
		 * 
		 * todoJPAService.retrieveTodos("Ranga") .forEach(todo ->
		 * log.info(todo.toString()));
		 * 
		 */
	}

	private void runAllStudentExamples() {
		Passport passport = new Passport("L12344432",
				"India");

		Student student = createStudent("dummy@dummy.com",
				"Doe", passport);
		student = genericRepository.createStudent(student);

		Project project = new Project();
		project.setName("Project1");

		project = genericRepository.createProject(project);

		genericRepository.assignStudentToProject(
				student.getId(), project.getId());

		Task task = new Task();
		task.setName("Task1");
		task.setProject(project);
		task.setStudent(student);
		genericRepository.createTask(task);

		Student student2 = studentRepository
				.retrieveStudent(101);
		System.out.println("student2 " + student2);

		printAllDataAfterTest();

		Passport passport2 = genericRepository
				.getPassport(201);
		System.out.println("passport 2 " + passport2);
		System.out.println("passport 2 Student"
				+ passport2.getStudent());
	}

	private Student createStudent(String email, String name,
			Passport passport) {
		Student student = new Student();
		student.setEmail(email);
		student.setName(name);
		student.setPassportId(passport);
		return student;
	}

	public void printAllDataAfterTest() {
		System.out.println(
				studentRepository.retrieveAllStudents());
	}
}
```
\src\main\java\com\example\demo\entity\Address.java
```java
package com.example.demo.entity;

import javax.persistence.Embeddable;

@Embeddable
public class Address {
	private String street;
	private String city;
	private String state;
	private String zip;
}
```
\src\main\java\com\example\demo\entity\Employee.java
```java
package com.example.demo.entity;

import javax.persistence.DiscriminatorColumn;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Inheritance;
import javax.persistence.InheritanceType;

//@MappedSuperclass
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
@DiscriminatorColumn(name = "disc_type")
public abstract class Employee {
	@Id
	protected Integer employeeId;
}
```
\src\main\java\com\example\demo\entity\FullTimeEmployee.java
```java
package com.example.demo.entity;

import javax.persistence.Entity;

@Entity
public class FullTimeEmployee extends Employee {
	protected Integer salary;
}
```
\src\main\java\com\example\demo\entity\PartTimeEmployee.java
```java
package com.example.demo.entity;

import javax.persistence.Entity;

@Entity
public class PartTimeEmployee extends Employee {
	protected Float hourlyWage;
}
```
\src\main\java\com\example\demo\entity\Passport.java
```java
package com.example.demo.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.OneToOne;

@Entity
public class Passport {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;

	private String number;

	@Column(name = "issued_country")
	private String issuedCountry;

	// Inverse Relationship
	// bi-directional OneToOne relationship
	// Column will not be created in the table
	// Try removing mappedBy = "passport" => You will see that student_id column
	// will be created in passport
	@OneToOne(fetch = FetchType.LAZY, mappedBy = "passport")
	private Student student;

	public Passport() {
		super();
	}

	public Passport(String number, String issuedCountry) {
		super();
		this.number = number;
		this.issuedCountry = issuedCountry;
	}

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getNumber() {
		return number;
	}

	public void setNumber(String number) {
		this.number = number;
	}

	public String getIssuedCountry() {
		return issuedCountry;
	}

	public void setIssuedCountry(String issuedCountry) {
		this.issuedCountry = issuedCountry;
	}

	public Student getStudent() {
		return student;
	}

	public void setStudent(Student student) {
		this.student = student;
	}

	@Override
	public String toString() {
		return "Passport [id=" + id + ", number=" + number
				+ ", issuedCountry=" + issuedCountry
				+ ", student=" + student + "]";
	}

}
```
\src\main\java\com\example\demo\entity\Project.java
```java
package com.example.demo.entity;

import java.util.List;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToMany;
import javax.persistence.OneToMany;

@Entity
public class Project {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;

	private String name;

	@OneToMany(mappedBy = "project")
	private List<Task> tasks;

	// There are some important differences between this many-to-many
	// relationship and the one-to-many relationship discussed earlier. The
	// first is a mathematical inevitability: when a many-to-many relationship
	// is bidirectional, both sides of the relationship are many-to-many
	// mappings.
	// The second difference is that there are no join columns on either side of
	// the relationship. You will see in the next section that the only way to
	// implement a many-to-many relationship is with a separate join table. The
	// consequence of not having any join columns in either of the entity tables
	// is that there is no way to determine which side is the owner of the
	// relationship. Because every bidirectional relationship has to have both
	// an owning side and an inverse side, we must pick one of the two entities
	// to be the owner. In this example, we picked Employee to be owner of the
	// relationship, but we could have just as easily picked Project instead. As
	// in every other bidirectional relationship, the inverse side must use the
	// mappedBy element to identify the owning attribute.
	// Note that no matter which side is designated as the owner, the other side
	// should include the mappedBy element; otherwise, the provider will think
	// that both sides are the owner and that the mappings are separate
	// unidirectional relationships.
	@ManyToMany
	// @JoinTable(name="STUDENT_PROJ",
	// joinColumns=@JoinColumn(name="STUDENT_ID"),
	// inverseJoinColumns=@JoinColumn(name="PROJECT_ID"))
	private List<Student> students;

	public Project() {
		super();
	}

	public List<Task> getTasks() {
		return tasks;
	}

	public void setTasks(List<Task> tasks) {
		this.tasks = tasks;
	}

	public List<Student> getStudents() {
		return students;
	}

	public void setStudents(List<Student> students) {
		this.students = students;
	}

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "Project [id=" + id + ", name=" + name + "]";
	}

}
```
\src\main\java\com\example\demo\entity\Student.java
```java
package com.example.demo.entity;

import java.util.List;

import javax.persistence.CascadeType;
import javax.persistence.Embedded;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToMany;
import javax.persistence.NamedQuery;
import javax.persistence.OneToMany;
import javax.persistence.OneToOne;
import javax.persistence.Table;

@Entity
@Table(name = "Student")
@NamedQuery(query = "select s from Student s", name = "find all students")
// SELECT p.number FROM Employee e JOIN e.phones p WHERE e.department.name =
// 'NA42' AND p.type = 'Cell'

// Group ing
// SELECT d, COUNT(e), MAX(e.salary), AVG(e.salary)
// FROM Department d JOIN d.employees e
// GROUP BY d
// HAVING COUNT(e) >= 5

// Query Params
// SELECT e
// FROM Employee e
// WHERE e.department = :dept AND
// e.salary > :base
// em.createQuery(QUERY, Long.class).setParameter("deptName",
// deptName).getSingleResult()

// Read only queries ->
// @TransactionAttribute(TransactionAttributeType.NOT_SUPPORTED)

// Criteria criteria = session.createCriteria(Book1.class)
// .add(Restrictions.eq("name", "Book 1"));
// List<Book1> books=criteria.list();

// Criterion nameRest = Restrictions.eq("name", "Hibernate
// Recipes").ignoreCase();
// gt (greater than), lt (less than), ge (greater than or equal to), isNull,
// isNotNull, isEmpty, isNotEmpty, between, in, le (less than or equal to)
// .add(Restrictions.like("name", "%Hibernate%"))
// .add(Restrictions.between("price", new Integer(100), new Integer(200)));
// .add(Restrictions.or(
// Restrictions.like("name", "%Hibernate%"),
// Restrictions.like("name", "%Java%")
// )
// )

// Criteria criteria = session.createCriteria(Book.class)
// .add(Restrictions.like("name", "%Hibernate%"))
// .createCriteria("publisher")
// .add(Restrictions.eq("name", "Manning"));
// List books = criteria.list();
// .addOrder(Order.asc("name"))
// .addOrder(Order.desc("publishDate"));

// .setProjection(Projections.projectionList()
// .add(Projections.groupProperty("publishDate"))
// .add(Projections.avg("price")));

// @MappedSuperclass
// public class Auditable {
// @Getter
// @Setter
// @Temporal(TemporalType.TIMESTAMP)
// Date createDate;
// }

// public class AuditableInterceptor extends EmptyInterceptor {
// @Override
// public boolean onSave(Object entity, Serializable id,
// Object[] state, String[] propertyNames,
// Type[] types) {
// if (entity instanceof Auditable) {
// for(int i=0;i<propertyNames.length;i++) {
// if("createDate".equals(propertyNames[i])) {
// state[i]=new Date();
// return true;
// }
// }
// }
// return false;
// }
// }

// Add interceptor to embeddedble context?

// Hibernate has a two-level cache architecture:

// The first-level cache is the persistence context cache, which is at the
// unit-of-work level. It corresponds to one session in Hibernate for a single
// request and is enabled by default for the Hibernate session.
// The second-level cache, which is either at the process scope or the cluster
// scope, is the cache of the state of the persistence entities. A
// cache-concurrency strategy defines the transaction isolation details for a
// particular item of data, and the cache provider represents the physical cache
// implementation.

public class Student {

	// ENUM
	// @Enumerated(EnumType.STRING)
	// private StudentType type;

	// @Transient
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;

	private String name;

	@OneToOne(cascade = { CascadeType.ALL })
	private Passport passport;

	private String email;

	@OneToMany(mappedBy = "student")
	private List<Task> tasks;

	@ManyToMany(mappedBy = "students")
	private List<Project> projects;

	@Embedded
	private Address address;

	// @ElementCollection
	// @CollectionTable(name="EMP_PHONE")
	// @MapKeyEnumerated(EnumType.STRING)
	// @MapKeyColumn(name="PHONE_TYPE")
	// @Column(name="PHONE_NUM")
	// private Map<PhoneType, String> phoneNumbers;
	// public enum PhoneType { Home, Mobile, Work }

	public List<Task> getTasks() {
		return tasks;
	}

	public void setTasks(List<Task> tasks) {
		this.tasks = tasks;
	}

	public void setPassport(Passport passport) {
		this.passport = passport;
	}

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Passport getPassport() {
		return passport;
	}

	public void setPassportId(Passport passport) {
		this.passport = passport;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name
				+ ", email=" + email + "]";
	}

}
```
\src\main\java\com\example\demo\entity\Task.java
```java
package com.example.demo.entity;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToOne;

@Entity
public class Task {

	public Task() {
		super();
	}

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;

	private String name;

	@ManyToOne
	// @JoinColumn(name="PROJECT_ID")
	private Project project;

	@ManyToOne
	private Student student;

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public Project getProject() {
		return project;
	}

	public void setProject(Project project) {
		this.project = project;
	}

	public Student getStudent() {
		return student;
	}

	public void setStudent(Student student) {
		this.student = student;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "Task [id=" + id + ", name=" + name + "]";
	}
}
```
\src\main\java\com\example\demo\entity\Todo.java
```java
package com.example.demo.entity;

import java.util.Date;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.NamedQuery;
import javax.persistence.Table;

@Entity
@Table(name = "Todo")
@NamedQuery(query = "select t from Todo t where user=?", name = "find all todos for user")
public class Todo {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private int id;

	private String user;

	private String desc;

	private Date targetDate;

	private boolean isDone;

	public Todo() {
		super();
	}

	public Todo(int id, String user, String desc,
			Date targetDate, boolean isDone) {
		super();
		this.id = id;
		this.user = user;
		this.desc = desc;
		this.targetDate = targetDate;
		this.isDone = isDone;
	}

	public Todo(String user, String desc, Date targetDate,
			boolean isDone) {
		super();
		this.user = user;
		this.desc = desc;
		this.targetDate = targetDate;
		this.isDone = isDone;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getUser() {
		return user;
	}

	public void setUser(String user) {
		this.user = user;
	}

	public String getDesc() {
		return desc;
	}

	public void setDesc(String desc) {
		this.desc = desc;
	}

	public Date getTargetDate() {
		return targetDate;
	}

	public void setTargetDate(Date targetDate) {
		this.targetDate = targetDate;
	}

	public boolean isDone() {
		return isDone;
	}

	public void setDone(boolean isDone) {
		this.isDone = isDone;
	}

	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + id;
		return result;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Todo other = (Todo) obj;
		if (id != other.id)
			return false;
		return true;
	}

	@Override
	public String toString() {
		return String.format(
				"Todo [id=%s, user=%s, desc=%s, targetDate=%s, isDone=%s]",
				id, user, desc, targetDate, isDone);
	}

}
```
\src\main\java\com\example\demo\rest\TodoRestService.java
```java
package com.example.demo.rest;

import java.net.URI;
import java.sql.SQLException;
import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.support.ServletUriComponentsBuilder;

import com.example.demo.data.todo.TodoJPAService;
import com.example.demo.entity.Todo;

@RestController
public class TodoRestService {

	@Autowired
	TodoJPAService todoJPAService;

	@GetMapping("/todos/user/{user}")
	public List<Todo> retrieveTodos(
			@PathVariable String user) throws SQLException {
		return todoJPAService.retrieveTodos(user);
	}

	@PostMapping("/todos")
	public ResponseEntity<Void> addTodo(
			@RequestBody Todo todo) throws SQLException {
		int todoID = todoJPAService.addTodo(todo.getUser(),
				todo.getDesc(), todo.getTargetDate(),
				todo.isDone());

		URI location = ServletUriComponentsBuilder
				.fromCurrentRequest().path("/{id}")
				.buildAndExpand(todoID).toUri();

		return ResponseEntity.created(location).build();
	}

	@GetMapping("/todos/{id}")
	public Todo retrieveTodo(@PathVariable int id)
			throws SQLException {
		return todoJPAService.retrieveTodo(id);
	}

	@PutMapping("/todos")
	public void updateTodo(@RequestBody Todo todo)
			throws SQLException {
		todoJPAService.updateTodo(todo);
	}

	@DeleteMapping("/todos/{id}")
	public void deleteTodo(@PathVariable int id)
			throws SQLException {
		todoJPAService.deleteTodo(id);
	}
}
```
\src\main\resources\application.properties
```properties
#logging.level.: DEBUG
spring.h2.console.enabled: true

# Log everything. Good for troubleshooting  
#logging.level.org.hibernate=debug
#logging.level.org.springframework=debug
#logging.level.javax.servlet=info
   
# Log all JDBC parameters  
#logging.level.org.hibernate.type=all
```
\src\main\resources\data.sql
```sql
INSERT INTO passport(id,number,issued_country)
VALUES (201,'L1234567','India');
INSERT INTO passport(id,number,issued_country)
VALUES (202,'L1234568','India');


INSERT INTO student(id, name, passport_id, email ) 
VALUES (101,'Jane', 201, 'jane@doe.com');
INSERT INTO student(id, name, passport_id, email )
VALUES (102,'Doe', 202, 'doe@doe.com');
```
\src\main\resources\schema-old.sql
```
DROP TABLE todo IF EXISTS;
/* NOT NEEDED WHEN JPA IS ACTIVE
CREATE TABLE todo(
id SERIAL auto_increment primary key, 
user VARCHAR(255), 
desc VARCHAR(255), 
target_date TIMESTAMP, 
is_done BOOLEAN);
*/
```
\src\test\java\com\example\demo\DemoApplicationTests.java
```java
package com.example.demo;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
public class DemoApplicationTests {

	@Test
	public void contextLoads() {
	}

}
```
