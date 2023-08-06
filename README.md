//# sunbase
// 1. Authenticate user and get Bearer token:
async function authenticateUser() {
  const apiUrl = 'https://qa2.sunbasedata.com/sunbase/portal/api/assignment_auth.jsp';
  const credentials = {
    login_id: 'test@sunbasedata.com',
    password: 'Test@123'
  };

  try {
    const response = await fetch(apiUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(credentials)
    });

    if (!response.ok) {
      throw new Error('Authentication failed');
    }

    const data = await response.json();
    const token = data.token; // Assuming the API response has the token field.
    return token;
  } catch (error) {
    console.error('Authentication error:', error.message);
    throw error; // Rethrow the error to handle it in the calling code.
  }
}

// 2. Create a new customer:
async function createCustomer(token) {
  const apiUrl = 'https://qa2.sunbasedata.com/sunbase/portal/api/assignment.jsp';
  const customerData = {
    first_name: 'Jane',
    last_name: 'Doe',
    street: 'Elvnu Street',
    address: 'H no 2',
    city: 'Delhi',
    state: 'Delhi',
    email: 'sam@gmail.com',
    phone: '12345678'
  };

  try {
    const response = await fetch(apiUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify({
        cmd: 'create',
        ...customerData
      })
    });

    if (response.status === 201) {
      console.log('Customer created successfully');
    } else if (response.status === 400) {
      const errorData = await response.json();
      throw new Error(`Failed to create customer: ${errorData.message}`);
    } else {
      throw new Error('Failed to create customer');
    }
  } catch (error) {
    console.error('Create customer error:', error.message);
    throw error; // Rethrow the error to handle it in the calling code.
  }
}

// 3. Get customer list:
async function getCustomerList(token) {
  const apiUrl = 'https://qa2.sunbasedata.com/sunbase/portal/api/assignment.jsp?cmd=get_customer_list';

  try {
    const response = await fetch(apiUrl, {
      method: 'GET',
      headers: {
        'Authorization': `Bearer ${token}`
      }
    });

    if (!response.ok) {
      throw new Error('Failed to get customer list');
    }

    const data = await response.json();
    console.log('Customer list:', data);
  } catch (error) {
    console.error('Get customer list error:', error.message);
    throw error; // Rethrow the error to handle it in the calling code.
  }
}

// 4. Delete a customer:
async function deleteCustomer(token, uuid) {
  const apiUrl = 'https://qa2.sunbasedata.com/sunbase/portal/api/assignment.jsp';
  const deleteData = {
    cmd: 'delete',
    uuid: uuid
  };

  try {
    const response = await fetch(apiUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify(deleteData)
    });

    if (response.status === 200) {
      console.log('Customer deleted successfully');
    } else if (response.status === 400) {
      const errorData = await response.json();
      throw new Error(`Failed to delete customer: ${errorData.message}`);
    } else if (response.status === 500) {
      throw new Error('Error: Customer not deleted');
    } else {
      throw new Error('Failed to delete customer');
    }
  } catch (error) {
    console.error('Delete customer error:', error.message);
    throw error; // Rethrow the error to handle it in the calling code.
  }
}

// 5. Update a customer:
async function updateCustomer(token, uuid) {
  const apiUrl = 'https://qa2.sunbasedata.com/sunbase/portal/api/assignment.jsp';
  const customerData = {
    first_name: 'Jane',
    last_name: 'Doe',
    street: 'Elvnu Street',
    address: 'H no 2',
    city: 'Delhi',
    state: 'Delhi',
    email: 'sam@gmail.com',
    phone: '12345678'
  };

  try {
    const response = await fetch(apiUrl, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${token}`
      },
      body: JSON.stringify({
        cmd: 'update',
        uuid: uuid,
        ...customerData
      })
    });

    if (response.status === 200) {
      console.log('Customer updated successfully');
    } else if (response.status === 400) {
      const errorData = await response.json();
      throw new Error(`Failed to update customer: ${errorData.message}`);
    } else {
      throw new Error('Failed to update customer');
    }
  } catch (error) {
    console.error('Update customer error:', error.message);
    throw error; // Rethrow the error to handle it in the calling code.
  }
}
